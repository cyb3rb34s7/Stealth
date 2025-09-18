# Error Muting & Analytics System - Detailed Implementation Plan

## Task Definition

**Problem:**
- Repetitive error notifications causing alert fatigue (500+ identical emails)
- Need for real-time error analytics and reporting capabilities
- Lack of intelligent error suppression mechanisms

**Solution:**
Implement a hybrid error tracking system that provides:
1. **Intelligent error muting** with time-based suppression
2. **Real-time analytics** from PostgreSQL for quick insights
3. **Detailed operational analysis** from structured logs
4. **Escalation notifications** for high-frequency error patterns

---

## Architecture Overview

### System Components:
```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   CMS Services  │───▶│ Monitoring API   │───▶│   External APIs │
│ (Error Sources) │    │ (Enhanced)       │    │ (Argus/Email)   │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                              │
                              ▼
                       ┌──────────────────┐
                       │ Error Tracking   │
                       │ (PostgreSQL +    │
                       │  Caffeine Cache) │
                       └──────────────────┘
                              │
                              ▼
                       ┌──────────────────┐
                       │  Analytics APIs  │
                       │ (Dashboard Data) │
                       └──────────────────┘
```

### Data Flow:
```
1. Error Request → Hash Generation → Cache Check → Database Operation → Mute Decision
2. If Allowed → Process Alarm/Notification → Update Analytics
3. If Muted → Log Decision → Increment Counter
4. Scheduled Job → Check Expired Mutes → Send Escalation Summaries
```

---

## Database Design

### Table: `error_tracking`

```sql
CREATE TABLE error_tracking (
    -- Primary identification
    id BIGSERIAL PRIMARY KEY,
    error_hash VARCHAR(64) UNIQUE NOT NULL,
    
    -- Error classification
    source_service VARCHAR(50) NOT NULL,
    error_type VARCHAR(100) NOT NULL,
    error_message TEXT NOT NULL,
    
    -- Muting logic
    muted_until TIMESTAMP NULL,
    
    -- Analytics tracking
    first_seen TIMESTAMP NOT NULL DEFAULT NOW(),
    last_seen TIMESTAMP NOT NULL DEFAULT NOW(),
    occurrence_count INTEGER NOT NULL DEFAULT 1,
    
    -- Time bucketing for aggregations
    hour_bucket TIMESTAMP NOT NULL,
    day_bucket DATE NOT NULL,
    
    -- Escalation tracking (Future Phase)
    escalation_sent BOOLEAN DEFAULT FALSE,
    escalation_sent_at TIMESTAMP NULL,
    
    -- Metadata
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Performance indexes
CREATE INDEX idx_error_hash ON error_tracking(error_hash);
CREATE INDEX idx_muted_until ON error_tracking(muted_until) WHERE muted_until IS NOT NULL;
CREATE INDEX idx_service_time ON error_tracking(source_service, last_seen);
CREATE INDEX idx_analytics ON error_tracking(source_service, error_type, day_bucket);
CREATE INDEX idx_hour_bucket ON error_tracking(hour_bucket);
CREATE INDEX idx_cleanup ON error_tracking(updated_at) WHERE muted_until IS NULL OR muted_until < NOW();
```

### Column Reasoning & Sample Values:

| Column | Purpose | Sample Value | Reasoning |
|--------|---------|--------------|-----------|
| `id` | Primary key | `12345` | Auto-incrementing for relationships |
| `error_hash` | Unique error identifier | `a1b2c3d4e5f6...` | SHA-256 of service+type+message |
| `source_service` | Originating service | `CMS-ADMIN` | Identifies error source for analytics |
| `error_type` | Error classification | `INTERNAL_SERVER_ERROR` | Groups similar error categories |
| `error_message` | Full error details | `Database connection timeout after 30s` | Complete context for debugging |
| `muted_until` | Mute expiration | `2025-09-09 15:30:00` | NULL means not muted |
| `first_seen` | Initial occurrence | `2025-09-09 14:30:00` | Track error lifecycle |
| `last_seen` | Latest occurrence | `2025-09-09 15:25:00` | Update on each occurrence |
| `occurrence_count` | Total occurrences | `47` | For analytics and escalation logic |
| `hour_bucket` | Hourly aggregation | `2025-09-09 15:00:00` | Fast time-series queries |
| `day_bucket` | Daily aggregation | `2025-09-09` | Daily/weekly report generation |
| `escalation_sent` | Escalation status | `false` | Prevent duplicate escalations |

---

## Project Structure

```
src/main/java/com/cms_monitoring_system/
├── alerts/
│   ├── controller/
│   │   └── AlertController.java              # Existing - Add muting logic
│   ├── dto/
│   │   ├── AlertRequestDto.java             # Existing - No changes needed
│   │   └── AlertResponseDto.java            # Existing - No changes needed
│   └── service/
│       └── AlertService.java                # Existing - Add muting integration
├── tracking/                                 # NEW PACKAGE
│   ├── dto/
│   │   ├── ErrorTrackingDto.java            # Database table representation
│   │   ├── MuteDecision.java                # Muting logic result DTO
│   │   ├── MuteStatus.java                  # Cache status DTO
│   │   └── AnalyticsResponseDto.java        # Analytics query results
│   ├── mapper/
│   │   └── ErrorTrackingMapper.java         # MyBatis mapper interface
│   └── service/
│       ├── ErrorTrackingService.java        # Core muting & analytics logic
│       ├── ErrorHashService.java            # Hash generation utility
│       └── EscalationService.java           # Future: escalation notifications
├── analytics/                               # NEW PACKAGE
│   ├── controller/
│   │   └── AnalyticsController.java         # REST APIs for dashboard
│   ├── service/
│   │   └── AnalyticsService.java            # Analytics business logic
│   └── dto/
│       ├── ErrorStatsDto.java               # Error statistics response
│       ├── ServiceHealthDto.java            # Service health metrics
│       └── TimeSeriesDto.java               # Time-based analytics
├── cache/                                   # NEW PACKAGE
│   ├── config/
│   │   └── CacheConfig.java                 # Caffeine cache configuration
│   └── service/
│       └── MuteCacheService.java            # Cache management service
└── common/
    └── util/
        └── StructuredLogger.java            # Existing - Add muting logs

resources/
├── mapper/                                  # MyBatis XML mappings
│   ├── ErrorTrackingMapper.xml             # Database operations
│   └── AnalyticsMapper.xml                 # Analytics queries
└── application.properties                   # Configuration file
```

### Component Explanations:

**tracking package:** Core error tracking functionality with database operations using DTOs
**analytics package:** Dashboard APIs and business intelligence queries  
**cache package:** In-memory cache management for performance
**mapper XML files:** MyBatis query definitions for clean SQL separation

### Key DTOs Structure:

**ErrorTrackingDto.java** - Maps to error_tracking table:
```java
public class ErrorTrackingDto {
    private Long id;
    private String errorHash;
    private String sourceService;
    private String errorType;
    private String errorMessage;
    private LocalDateTime mutedUntil;
    private LocalDateTime firstSeen;
    private LocalDateTime lastSeen;
    private Integer occurrenceCount;
    private LocalDateTime hourBucket;
    private LocalDate dayBucket;
    private Boolean escalationSent;
    private LocalDateTime escalationSentAt;
    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;
    // constructors, getters, setters
}
```

**MuteDecision.java** - Result of muting logic:
```java
public class MuteDecision {
    private boolean muted;
    private LocalDateTime mutedUntil;
    private String reason;
    private Integer occurrenceCount;
    
    public static MuteDecision allow() {
        return new MuteDecision(false, null, "First occurrence or mute expired", 1);
    }
    
    public static MuteDecision mute(LocalDateTime until) {
        return new MuteDecision(true, until, "Recent similar error detected", null);
    }
    // constructors, getters, setters
}
```

**MuteStatus.java** - Cache representation:
```java
public class MuteStatus {
    private String errorHash;
    private boolean isMuted;
    private LocalDateTime mutedUntil;
    private Integer occurrenceCount;
    private LocalDateTime lastUpdated;
    
    public MuteDecision toMuteDecision() {
        if (isMuted && mutedUntil.isAfter(LocalDateTime.now())) {
            return MuteDecision.mute(mutedUntil);
        }
        return MuteDecision.allow();
    }
    // constructors, getters, setters
}
```

---

## Implementation Details

### New Components to Create

#### 1. ErrorTrackingService.java
```java
@Service
@Transactional
public class ErrorTrackingService {
    
    private final ErrorTrackingMapper errorTrackingMapper;
    private final MuteCacheService muteCacheService;
    private final ErrorHashService errorHashService;
    
    public MuteDecision checkAndUpdateErrorTracking(AlertRequestDto request) {
        String errorHash = errorHashService.generateHash(request);
        
        // Check cache first
        MuteStatus cachedStatus = muteCacheService.getMuteStatus(errorHash);
        if (cachedStatus != null) {
            return cachedStatus.toMuteDecision();
        }
        
        // Database operation with upsert logic
        ErrorTrackingDto record = errorTrackingMapper.findByHash(errorHash);
        MuteDecision decision = processErrorRecord(record, request, errorHash);
        
        // Update cache
        muteCacheService.cacheMuteStatus(errorHash, decision);
        
        return decision;
    }
    
    private MuteDecision processErrorRecord(ErrorTrackingDto existing, 
                                          AlertRequestDto request, String hash) {
        LocalDateTime now = LocalDateTime.now();
        
        if (existing == null) {
            // First occurrence - always allow
            createNewErrorRecord(request, hash, now);
            return MuteDecision.allow();
        }
        
        Duration timeSinceLastSeen = Duration.between(existing.getLastSeen(), now);
        
        if (timeSinceLastSeen.toMinutes() <= 5) {
            // Within 5 minutes - mute for 30 minutes
            updateExistingRecord(existing, now.plusMinutes(30));
            return MuteDecision.mute(now.plusMinutes(30));
        } else {
            // After 5 minutes - reset and allow
            resetMuteStatus(existing, now);
            return MuteDecision.allow();
        }
    }
}
```

#### 2. ErrorTrackingMapper.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.cms_monitoring_system.tracking.mapper.ErrorTrackingMapper">

    <insert id="insertOrUpdate" parameterType="ErrorTracking">
        INSERT INTO error_tracking (
            error_hash, source_service, error_type, error_message, 
            hour_bucket, day_bucket, first_seen, last_seen, occurrence_count
        ) VALUES (
            #{errorHash}, #{sourceService}, #{errorType}, #{errorMessage},
            DATE_TRUNC('hour', NOW()), CURRENT_DATE, NOW(), NOW(), 1
        )
        ON CONFLICT (error_hash) DO UPDATE SET
            last_seen = NOW(),
            occurrence_count = error_tracking.occurrence_count + 1,
            muted_until = CASE 
                WHEN NOW() - error_tracking.last_seen &lt; INTERVAL '5 minutes' 
                THEN NOW() + INTERVAL '30 minutes'
                ELSE NULL 
            END,
            updated_at = NOW()
        RETURNING *;
    </insert>

    <select id="findByHash" parameterType="string" resultType="ErrorTrackingDto">
        SELECT * FROM error_tracking 
        WHERE error_hash = #{errorHash}
    </select>

    <select id="getErrorStatsByService" resultType="ErrorStatsDto">
        SELECT 
            source_service,
            COUNT(DISTINCT error_type) as error_variety,
            SUM(occurrence_count) as total_errors,
            MAX(last_seen) as last_error_time
        FROM error_tracking 
        WHERE day_bucket >= CURRENT_DATE - INTERVAL '7 days'
        GROUP BY source_service
        ORDER BY total_errors DESC;
    </select>

</mapper>
```

#### 3. CacheConfig.java
```java
@Configuration
@EnableCaching
public class CacheConfig {

    @Bean
    public Cache<String, MuteStatus> muteCache() {
        return Caffeine.newBuilder()
            .maximumSize(10_000)
            .expireAfterWrite(30, TimeUnit.MINUTES)
            .recordStats()
            .build();
    }

    @Bean
    public CacheManager cacheManager() {
        CaffeineCacheManager cacheManager = new CaffeineCacheManager();
        cacheManager.setCaffeine(Caffeine.newBuilder()
            .maximumSize(10_000)
            .expireAfterWrite(30, TimeUnit.MINUTES));
        return cacheManager;
    }
}
```

#### 4. AnalyticsController.java
```java
@RestController
@RequestMapping("/cms/monitoring/analytics")
public class AnalyticsController {

    private final AnalyticsService analyticsService;

    @GetMapping("/errors/by-service")
    public ResponseDto<List<ErrorStatsDto>> getErrorsByService(
            @RequestParam(defaultValue = "7") int days) {
        List<ErrorStatsDto> stats = analyticsService.getErrorStatsByService(days);
        return ResponseHandler.processMethodResponse("success", stats);
    }

    @GetMapping("/trends/hourly")
    public ResponseDto<List<TimeSeriesDto>> getHourlyTrends(
            @RequestParam(defaultValue = "24") int hours) {
        List<TimeSeriesDto> trends = analyticsService.getHourlyErrorTrends(hours);
        return ResponseHandler.processMethodResponse("success", trends);
    }
}
```

### Existing Files to Modify

#### AlertController.java Changes:
```java
@PostMapping("/alarm")
public ResponseDto raiseAlarmAndNotification(@Valid @RequestBody AlertRequestDto request, 
                                           HttpServletRequest httpRequest) {
    setupTraceContext(request, httpRequest);
    
    // NEW: Check muting before processing
    MuteDecision muteDecision = errorTrackingService.checkAndUpdateErrorTracking(request);
    
    if (muteDecision.isMuted()) {
        StructuredLogger.logMutedRequest("Request muted until: " + muteDecision.getMutedUntil());
        return ResponseHandler.processMethodResponse(Constants.RESPONSE_KEY, 
            "Request muted due to recent similar errors");
    }
    
    // EXISTING: Continue with normal processing
    alertService.setupRequestContext("ALARM_REQUEST", request);
    // ... rest of existing logic
}
```

#### AlertService.java Changes:
```java
@Service
public class AlertService {
    
    // NEW: Inject error tracking service
    private final ErrorTrackingService errorTrackingService;
    
    // EXISTING: All current methods remain unchanged
    // NEW: Add muting context to logs
    public void raiseAlarm(AlertRequestDto request) {
        StructuredLogger.setRequestContext("ALARM_ATTEMPT", request.getSourceService(), 
            env, request.getRegion(), request.getErrorType(), request.getErrorMessage());
        
        RetryUtil.retry(() -> {
            raiseAlarmInternal(request);
            return true;
        });
        
        StructuredLogger.logOperationSuccess("Alarm attempt completed successfully");
    }
}
```

---

## Implementation Plan

### Phase 1: Core Infrastructure
1. **Database Setup**
   - Create migration script for `error_tracking` table
   - Set up MyBatis configuration and mappers
   - Test database connectivity and operations

2. **Error Tracking Service**
   - Implement `ErrorTrackingService` with basic muting logic
   - Create `ErrorHashService` for consistent hash generation
   - Add MyBatis mapper and XML configurations

3. **Cache Integration**
   - Configure Caffeine cache with appropriate settings
   - Implement `MuteCacheService` for cache operations
   - Add cache metrics and monitoring

### Phase 2: API Integration
1. **Modify Alert Controllers**
   - Add muting checks to all three endpoints (`/alarm`, `/notification`, `/alert`)
   - Ensure backward compatibility with existing functionality
   - Add proper error handling and logging

2. **Analytics APIs**
   - Create `AnalyticsController` with dashboard endpoints
   - Implement basic analytics queries (error counts, trends)
   - Add proper response formatting and error handling

### Phase 3: Testing & Validation
1. **Unit Testing**
   - Test muting logic with various time scenarios
   - Validate cache behavior and database operations
   - Test analytics query performance

2. **Integration Testing**
   - End-to-end testing with real error scenarios
   - Performance testing under high error volumes
   - Cache invalidation and consistency testing

---

## Expected Behavior

### Muting Logic Examples:

**Scenario 1: First Error**
- Input: CMS-ADMIN sends "Database timeout" error
- Expected: Error is processed normally, record created in DB
- Result: Alarm/notification sent, occurrence_count = 1

**Scenario 2: Repeated Error (Within 5 minutes)**
- Input: Same error from CMS-ADMIN within 5 minutes
- Expected: Error is muted for 30 minutes, count incremented
- Result: No alarm/notification, occurrence_count = 2, muted_until set

**Scenario 3: Error After Mute Period**
- Input: Same error after 30+ minutes
- Expected: Mute is reset, error processed normally
- Result: Alarm/notification sent, muted_until cleared

**Scenario 4: Different Error from Same Service**
- Input: CMS-ADMIN sends "Memory exhausted" error
- Expected: New error hash, processed normally
- Result: New record created, both errors tracked separately

### Analytics Behavior:

**Dashboard Queries:**
- Real-time error counts by service (sub-second response)
- Hourly error trends for charts
- Service health scoring based on error patterns
- Top error types and their frequency

**Performance Expectations:**
- Cache hit rate: >90% for muting decisions
- Database query time: <100ms for analytics
- API response time: <2 seconds for dashboard data

---

## Testing Scenarios

### Functional Testing:

1. **Muting Logic Tests:**
   ```
   Test Case 1: Verify first occurrence is allowed
   Test Case 2: Verify muting within 5-minute window
   Test Case 3: Verify mute expiration after 30 minutes
   Test Case 4: Verify different errors are tracked separately
   Test Case 5: Verify hash consistency across requests
   ```

2. **Cache Behavior Tests:**
   ```
   Test Case 1: Cache hit scenario (fast response)
   Test Case 2: Cache miss scenario (database fallback)
   Test Case 3: Cache expiration and refresh
   Test Case 4: Multi-container cache consistency
   ```

3. **Analytics Tests:**
   ```
   Test Case 1: Error count aggregation accuracy
   Test Case 2: Time-series data consistency
   Test Case 3: Service health calculation
   Test Case 4: Performance under load
   ```

### Performance Testing:

1. **High Volume Error Simulation:**
   - 1000 identical errors in 1 minute
   - Verify only first error is processed
   - Measure database and cache performance

2. **Multi-Service Error Load:**
   - Concurrent errors from multiple services
   - Verify proper isolation and tracking
   - Test cache efficiency and hit rates

3. **Analytics Query Performance:**
   - Dashboard load with 1M+ error records
   - Verify sub-second response times
   - Test index effectiveness

### Integration Testing:

1. **End-to-End Workflow:**
   - Real service error → Muting check → External API calls
   - Verify complete request lifecycle
   - Test error scenarios and rollback

2. **Database Consistency:**
   - Multiple containers processing same error
   - Verify UPSERT logic handles conflicts
   - Test transaction isolation

---

## MyBatis Implementation Guidelines

### Configuration Setup:
```yaml
# application.yml
mybatis:
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.cms_monitoring_system.tracking.entity
  configuration:
    map-underscore-to-camel-case: true
    cache-enabled: true
    lazy-loading-enabled: false
```

### Mapper Interface Pattern:
```java
@Mapper
public interface ErrorTrackingMapper {
    ErrorTracking findByHash(@Param("errorHash") String errorHash);
    void insertOrUpdate(ErrorTracking errorTracking);
    List<ErrorStatsDto> getErrorStatsByService(@Param("days") int days);
    List<TimeSeriesDto> getHourlyTrends(@Param("hours") int hours);
}
```

### XML Query Guidelines:
- Use parameterized queries to prevent SQL injection
- Implement proper result mapping for complex objects
- Use CDATA sections for complex SQL with operators
- Add query comments for maintenance clarity

---

## Development Guidelines

### Code Quality Standards:
1. **Follow Existing Patterns:**
   - Use same package structure conventions
   - Follow existing naming patterns for services and DTOs
   - Maintain consistent logging format and levels

2. **Preserve Current Functionality:**
   - Do not modify existing AlertService core logic
   - Keep current API response formats unchanged
   - Maintain backward compatibility for all endpoints

3. **Error Handling:**
   - Use existing CustomException patterns
   - Follow current retry mechanism implementation
   - Maintain consistent error response formats

4. **Logging Standards:**
   - Use StructuredLogger for all new logging
   - Follow existing MDC context patterns
   - Add muting-specific log messages with proper levels

### Database Development:
1. **Migration Scripts:**
   - Use Flyway migration naming conventions
   - Include rollback scripts for all changes
   - Test migrations on sample data

2. **Query Optimization:**
   - Use EXPLAIN ANALYZE for complex queries
   - Implement proper indexing strategy
   - Monitor query performance in development

### Testing Requirements:
1. **Unit Test Coverage:**
   - Minimum 80% code coverage for new components
   - Test edge cases and error scenarios
   - Mock external dependencies properly

2. **Integration Tests:**
   - Test database operations with real PostgreSQL
   - Verify cache behavior under load
   - Test complete request workflows

This implementation plan provides comprehensive guidance for building the error muting and analytics system while maintaining code quality and following established patterns.