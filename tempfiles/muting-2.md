# Error Muting & Escalation System - Final Implementation Plan

## Problem Statement

**Issue:**
- Repetitive error notifications causing alert fatigue (500+ identical emails from batch jobs/crons)
- Need for escalation summaries when errors are muted to avoid missing critical issues

**Solution:**
Direct database-driven error tracking with intelligent muting and escalation notifications using simple PostgreSQL operations and application-layer business logic.

---

## System Flow

1. **Error request arrives** → Generate hash from sourceService + errorType + errorMessage
2. **Query database** for existing record with that hash  
3. **Apply muting logic** in application layer based on timing rules
4. **Check escalation requirements** in application layer
5. **Update database** with new occurrence count and mute status
6. **Send escalation email** if mute period expired and not already sent
7. **Process or block** alarm/notification based on mute decision

---

## Database Schema

### Table: `error_tracking`

```sql
CREATE TABLE error_tracking (
    error_hash VARCHAR(64) PRIMARY KEY,
    source_service VARCHAR(50) NOT NULL,
    error_type VARCHAR(100) NOT NULL,
    error_message TEXT NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    last_error_at TIMESTAMP NOT NULL DEFAULT NOW(),
    muted_until TIMESTAMP NULL,
    occurrence_count INTEGER NOT NULL DEFAULT 1,
    escalation_sent BOOLEAN DEFAULT FALSE,
    escalation_sent_at TIMESTAMP NULL,
    hour_bucket TIMESTAMP NOT NULL,
    day_bucket DATE NOT NULL
);

-- Indexes
CREATE INDEX idx_muted_until ON error_tracking(muted_until) WHERE muted_until IS NOT NULL;
CREATE INDEX idx_escalation_check ON error_tracking(escalation_sent, muted_until) WHERE escalation_sent = FALSE;
CREATE INDEX idx_service_time ON error_tracking(source_service, last_error_at);
```

### Column Purpose:

| Column | Purpose | Business Logic |
|--------|---------|----------------|
| `error_hash` | Unique identifier (SHA-256 of service+type+message) | Primary key for deduplication |
| `source_service` | Service that generated error | For grouping and escalation emails |
| `error_type` | Error classification | For escalation email subject |
| `error_message` | Full error description | For escalation email content |
| `created_at` | First occurrence timestamp | Never changes, tracks error pattern start |
| `last_error_at` | Most recent occurrence | Updated each time, drives muting logic |
| `muted_until` | Mute expiration timestamp | NULL = not muted, future = muted until then |
| `occurrence_count` | Total error count | Incremented each occurrence, shown in escalation |
| `escalation_sent` | Escalation email status | Prevents duplicate escalation emails |
| `escalation_sent_at` | Escalation timestamp | Tracks when escalation was sent |
| `hour_bucket` | Hour truncation for future analytics | `DATE_TRUNC('hour', NOW())` |
| `day_bucket` | Date for future analytics | `CURRENT_DATE` |

---

## Implementation Structure

```
src/main/java/com/cms_monitoring_system/
├── alerts/
│   ├── controller/
│   │   └── AlertController.java              # MODIFY - Add muting checks
│   ├── dto/
│   │   ├── AlertRequestDto.java             # Existing - No changes
│   │   └── AlertResponseDto.java            # Existing - No changes  
│   └── service/
│       └── AlertService.java                # MODIFY - Add muting integration
├── tracking/                                 # NEW PACKAGE
│   ├── dto/
│   │   ├── ErrorTrackingDto.java            # Database representation
│   │   ├── MuteDecision.java                # Muting result
│   │   └── EscalationDecision.java          # Escalation result
│   ├── mapper/
│   │   └── ErrorTrackingMapper.java         # MyBatis mapper
│   └── service/
│       ├── ErrorTrackingService.java        # Core muting & escalation logic
│       ├── ErrorHashService.java            # Hash generation
│       └── EscalationService.java           # Escalation emails
└── common/
    └── util/
        └── StructuredLogger.java            # MODIFY - Add muting logs
```

---

## Core Implementation

### ErrorTrackingService.java
```java
@Service
@Transactional
public class ErrorTrackingService {
    
    private final ErrorTrackingMapper mapper;
    private final ErrorHashService hashService;
    private final EscalationService escalationService;
    
    public MuteDecision processErrorRequest(AlertRequestDto request) {
        String errorHash = hashService.generateHash(request);
        ErrorTrackingDto existing = mapper.findByHash(errorHash);
        
        // Calculate mute decision
        MuteDecision muteDecision = calculateMuteDecision(existing);
        
        // Check escalation before updating record
        EscalationDecision escalationDecision = checkEscalationNeeded(existing);
        
        // Update database
        updateErrorRecord(existing, request, muteDecision, errorHash);
        
        // Send escalation if needed
        if (escalationDecision.isNeeded()) {
            escalationService.sendEscalationEmail(existing);
            mapper.markEscalationSent(errorHash);
        }
        
        return muteDecision;
    }
    
    private MuteDecision calculateMuteDecision(ErrorTrackingDto existing) {
        LocalDateTime now = LocalDateTime.now();
        
        if (existing == null) {
            return MuteDecision.allow("First occurrence");
        }
        
        // Check if currently muted
        if (existing.getMutedUntil() != null && existing.getMutedUntil().isAfter(now)) {
            return MuteDecision.mute(existing.getMutedUntil(), "Still in mute period");
        }
        
        // Check if within 5-minute window
        Duration timeSinceLastError = Duration.between(existing.getLastErrorAt(), now);
        if (timeSinceLastError.toMinutes() <= 5) {
            LocalDateTime muteUntil = now.plusMinutes(30);
            return MuteDecision.mute(muteUntil, "Recent similar error - muting for 30 minutes");
        }
        
        return MuteDecision.allow("More than 5 minutes since last occurrence");
    }
    
    private EscalationDecision checkEscalationNeeded(ErrorTrackingDto existing) {
        if (existing == null || existing.getEscalationSent()) {
            return EscalationDecision.notNeeded("No record or already escalated");
        }
        
        LocalDateTime now = LocalDateTime.now();
        
        // Check if mute expired and escalation not sent
        if (existing.getMutedUntil() != null && 
            existing.getMutedUntil().isBefore(now) && 
            !existing.getEscalationSent()) {
            return EscalationDecision.needed("Mute period expired");
        }
        
        return EscalationDecision.notNeeded("Escalation conditions not met");
    }
    
    private void updateErrorRecord(ErrorTrackingDto existing, AlertRequestDto request, 
                                 MuteDecision decision, String errorHash) {
        if (existing == null) {
            mapper.insertNewError(errorHash, request.getSourceService(), 
                                request.getErrorType(), request.getErrorMessage(), 
                                decision.getMutedUntil());
        } else {
            mapper.updateExistingError(errorHash, decision.getMutedUntil(), 
                                     existing.getOccurrenceCount() + 1);
        }
    }
}
```

### EscalationService.java
```java
@Service
public class EscalationService {
    
    private final HttpMethodHandler httpMethodHandler;
    
    @Value("${CMS_NOTIFICATION_SVR_URL}")
    private String notificationServerUrl;
    
    @Value("${CMS_NOTIFICATION_API}")
    private String notificationApi;
    
    @Value("${env}")
    private String environment;
    
    public void sendEscalationEmail(ErrorTrackingDto errorRecord) {
        String subject = String.format("%s - ESCALATION: %s occurred %d times in %s", 
            environment.toUpperCase(),
            errorRecord.getErrorType(), 
            errorRecord.getOccurrenceCount(),
            errorRecord.getSourceService()
        );
        
        String message = String.format(
            "ERROR ESCALATION SUMMARY\n\n" +
            "Service: %s\n" +
            "Error Type: %s\n" +
            "Total Occurrences: %d\n" +
            "First Seen: %s\n" +
            "Last Seen: %s\n" +
            "Mute Period: 30 minutes\n\n" +
            "Error Details:\n%s\n\n" +
            "This error was automatically muted due to high frequency. " +
            "Please investigate the root cause to prevent future occurrences.",
            errorRecord.getSourceService(),
            errorRecord.getErrorType(),
            errorRecord.getOccurrenceCount(),
            errorRecord.getCreatedAt(),
            errorRecord.getLastErrorAt(),
            errorRecord.getErrorMessage()
        );
        
        NotificationDto notification = NotificationDto.builder()
            .messageSub(subject)
            .message(message)
            .build();
        
        try {
            httpMethodHandler.handleHttpExchange(
                notificationServerUrl + notificationApi,
                Constants.METHOD_POST,
                new HttpEntity<>(notification),
                ResponseEntity.class
            );
        } catch (Exception ex) {
            log.error("Failed to send escalation email for hash: {}", 
                     errorRecord.getErrorHash(), ex);
        }
    }
}
```

### ErrorHashService.java
```java
@Service
public class ErrorHashService {
    
    public String generateHash(AlertRequestDto request) {
        String combined = request.getSourceService() + "|" + 
                         request.getErrorType() + "|" + 
                         request.getErrorMessage();
        return DigestUtils.sha256Hex(combined);
    }
}
```

---

## Database Operations

### ErrorTrackingMapper.xml
```xml
<mapper namespace="com.cms_monitoring_system.tracking.mapper.ErrorTrackingMapper">

    <select id="findByHash" parameterType="string" resultType="ErrorTrackingDto">
        SELECT * FROM error_tracking WHERE error_hash = #{errorHash}
    </select>

    <insert id="insertNewError">
        INSERT INTO error_tracking (
            error_hash, source_service, error_type, error_message,
            created_at, last_error_at, occurrence_count, muted_until,
            hour_bucket, day_bucket
        ) VALUES (
            #{errorHash}, #{sourceService}, #{errorType}, #{errorMessage},
            NOW(), NOW(), 1, #{mutedUntil},
            DATE_TRUNC('hour', NOW()), CURRENT_DATE
        )
    </insert>

    <update id="updateExistingError">
        UPDATE error_tracking SET
            last_error_at = NOW(),
            occurrence_count = #{occurrenceCount},
            muted_until = #{mutedUntil},
            escalation_sent = CASE 
                WHEN #{mutedUntil} IS NULL THEN FALSE 
                ELSE escalation_sent 
            END
        WHERE error_hash = #{errorHash}
    </update>

    <update id="markEscalationSent">
        UPDATE error_tracking SET
            escalation_sent = TRUE,
            escalation_sent_at = NOW()
        WHERE error_hash = #{errorHash}
    </update>

</mapper>
```

---

## Controller Integration

### AlertController.java Changes
```java
@PostMapping("/alarm")
public ResponseDto raiseAlarmAndNotification(@Valid @RequestBody AlertRequestDto request, 
                                           HttpServletRequest httpRequest) {
    setupTraceContext(request, httpRequest);
    
    // Check muting and escalation
    MuteDecision muteDecision = errorTrackingService.processErrorRequest(request);
    
    if (muteDecision.isMuted()) {
        StructuredLogger.logMutedRequest(muteDecision.getReason());
        return ResponseHandler.processMethodResponse(Constants.RESPONSE_KEY, 
            "Request muted - similar error occurred recently");
    }
    
    // Process normally if not muted
    alertService.setupRequestContext("ALARM_REQUEST", request);
    String resultMessage = alertService.raiseAlarmAndNotification(request);
    return ResponseHandler.processMethodResponse(Constants.RESPONSE_KEY, resultMessage);
}

@PostMapping("/notification")  
public ResponseDto sendNotification(@Valid @RequestBody AlertRequestDto request,
                                  HttpServletRequest httpRequest) {
    setupTraceContext(request, httpRequest);
    
    // Check muting and escalation
    MuteDecision muteDecision = errorTrackingService.processErrorRequest(request);
    
    if (muteDecision.isMuted()) {
        StructuredLogger.logMutedRequest(muteDecision.getReason());
        return ResponseHandler.processMethodResponse(Constants.RESPONSE_KEY, 
            "Notification muted - similar error occurred recently");
    }
    
    // Process normally if not muted
    alertService.setupRequestContext("NOTIFICATION_REQUEST", request);
    alertService.sendNotification(request);
    return ResponseHandler.processMethodResponse(Constants.RESPONSE_KEY,
        Constants.SUCCESS_MESSAGE_NOTIFICATION);
}
```

---

## Key DTOs

### MuteDecision.java
```java
public class MuteDecision {
    private boolean muted;
    private LocalDateTime mutedUntil;
    private String reason;
    
    public static MuteDecision allow(String reason) {
        return new MuteDecision(false, null, reason);
    }
    
    public static MuteDecision mute(LocalDateTime until, String reason) {
        return new MuteDecision(true, until, reason);
    }
}
```

### EscalationDecision.java
```java
public class EscalationDecision {
    private boolean needed;
    private String reason;
    
    public static EscalationDecision needed(String reason) {
        return new EscalationDecision(true, reason);
    }
    
    public static EscalationDecision notNeeded(String reason) {
        return new EscalationDecision(false, reason);
    }
}
```

---

## Configuration

### application.properties
```properties
# Database
spring.datasource.url=jdbc:postgresql://localhost:5432/cms_monitoring
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}

# MyBatis
mybatis.mapper-locations=classpath:mapper/*.xml
mybatis.type-aliases-package=com.cms_monitoring_system.tracking.dto
mybatis.configuration.map-underscore-to-camel-case=true

# Existing notification settings
CMS_NOTIFICATION_SVR_URL=${CMS_NOTIFICATION_SVR_URL}
CMS_NOTIFICATION_API=${CMS_NOTIFICATION_API}
env=${env}
```

---

## Implementation Steps

1. **Create error_tracking table** with all columns and indexes
2. **Implement ErrorTrackingService** with muting and escalation logic
3. **Create EscalationService** with different email template
4. **Set up MyBatis mappers** with simple CRUD operations
5. **Modify AlertController** to integrate muting checks
6. **Update StructuredLogger** for muting-related logs
7. **Test complete flow** with various error scenarios

This implementation solves alert fatigue with intelligent muting and ensures critical issues aren't missed through escalation emails with different subject/body formatting.