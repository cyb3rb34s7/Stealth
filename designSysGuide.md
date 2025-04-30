# HTMS Design System

## Version 1.0 - May 2025

---

## Introduction

The HTMS Design System provides a comprehensive framework of components, patterns, and guidelines that ensure consistency, efficiency, and quality across the HTMS platform. This system serves as the single source of truth for design decisions and implementation specifications.

### Purpose

- Establish a consistent visual language across the platform
- Accelerate design and development with pre-built components
- Ensure accessibility and usability standards are met
- Facilitate collaboration between design and development teams
- Enable scalable and maintainable frontend architecture

### Design Principles

1. **Intelligence with Clarity**: Powerful capabilities presented with visual simplicity
2. **Responsive Fluidity**: Adaptive layouts with smooth transitions
3. **Purposeful Animation**: Motion that enhances understanding and delight
4. **Consistent Patterns**: Familiar interactions that reduce cognitive load
5. **Accessible by Design**: Inclusive experiences for all users

---

## Foundation Elements

### Color System

#### Primary Brand Colors

| Name | Value | Token | Usage |
|------|-------|-------|-------|
| HTMS Blue | #3d5afe | `--color-primary` | Primary actions, brand elements, key indicators |
| HTMS Blue Light | #8187ff | `--color-primary-light` | Hover states, secondary elements, backgrounds |
| HTMS Blue Dark | #0031ca | `--color-primary-dark` | Active states, text on light backgrounds |

#### Secondary Colors

| Name | Value | Token | Usage |
|------|-------|-------|-------|
| HTMS Purple | #7c4dff | `--color-secondary` | AI-powered features, intelligence indicators |
| HTMS Purple Light | #b47cff | `--color-secondary-light` | AI feature backgrounds, hover states |
| HTMS Purple Dark | #3f1dcb | `--color-secondary-dark` | AI feature active states |

#### Functional Colors

| Name | Value | Token | Usage |
|------|-------|-------|-------|
| Success | #00e676 | `--color-success` | Positive actions, completion, approval |
| Warning | #ff9100 | `--color-warning` | Caution, pending, in-progress |
| Error | #ff1744 | `--color-error` | Errors, blockers, high priority |
| Info | #2979ff | `--color-info` | Informational elements, help |

#### Neutral Colors

| Name | Value | Token | Usage |
|------|-------|-------|-------|
| Black | #263238 | `--color-neutral-darker` | Primary text |
| Dark Gray | #37474f | `--color-neutral-dark` | Secondary text, headers |
| Medium Gray | #546e7a | `--color-neutral-medium` | Tertiary text, borders |
| Light Gray | #b0bec5 | `--color-neutral-light` | Disabled text, subtle elements |
| Lighter Gray | #eceff1 | `--color-neutral-lighter` | Backgrounds, dividers |
| White | #ffffff | `--color-neutral-white` | Card backgrounds, text on dark backgrounds |

#### Gradient Definitions

| Name | Definition | Token | Usage |
|------|------------|-------|-------|
| Blue Gradient | linear-gradient(to right, #2979ff, #448aff) | `--gradient-primary` | Call-to-action buttons, emphasis |
| Purple Gradient | linear-gradient(to right, #7c4dff, #b388ff) | `--gradient-secondary` | AI elements, assistant functions |
| Green Gradient | linear-gradient(to right, #00e676, #69f0ae) | `--gradient-success` | Success states, completion |
| Orange Gradient | linear-gradient(to right, #ff9100, #ffab40) | `--gradient-warning` | Warning states, attention needed |
| Red Gradient | linear-gradient(to right, #ff1744, #ff5252) | `--gradient-error` | Error states, critical issues |

#### Color Usage Guidelines

1. **Accessibility**
   - Ensure text meets WCAG 2.1 AA contrast requirements (4.5:1 for normal text, 3:1 for large text)
   - Never use color as the sole means of conveying information
   - Provide additional indicators (icons, patterns) alongside color coding

2. **Hierarchy**
   - Use primary colors sparingly for key actions and focal points
   - Employ neutral colors for most UI elements and text
   - Reserve functional colors for their specific semantic meanings

3. **Background Combinations**
   - Dark text (#263238, #37474f) on light backgrounds
   - Light text (#eceff1, #ffffff) on dark backgrounds
   - Avoid vibrant backgrounds for large areas

### Typography

#### Font Family

```css
--font-family-primary: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
--font-family-monospace: SFMono-Regular, Consolas, "Liberation Mono", Menlo, monospace;
```

#### Type Scale

| Name | Size | Line Height | Weight | Token | Usage |
|------|------|-------------|--------|-------|-------|
| Display 1 | 34px | 40px | 700 | `--text-display-1` | Page headers |
| Display 2 | 28px | 34px | 700 | `--text-display-2` | Section headers |
| Heading 1 | 22px | 28px | 600 | `--text-heading-1` | Card titles, prominent headings |
| Heading 2 | 18px | 24px | 600 | `--text-heading-2` | Subsection headers |
| Body 1 | 16px | 22px | 400 | `--text-body-1` | Primary content |
| Body 2 | 14px | 20px | 400 | `--text-body-2` | Secondary content, UI elements |
| Caption | 12px | 16px | 400 | `--text-caption` | Labels, supporting text |
| Overline | 10px | 14px | 500 | `--text-overline` | Small labels, metadata |

#### Font Weight

```css
--font-weight-regular: 400;
--font-weight-medium: 500;
--font-weight-semibold: 600;
--font-weight-bold: 700;
```

#### Typography Usage Guidelines

1. **Readability**
   - Maintain minimum 14px for body text
   - Ensure sufficient contrast with backgrounds
   - Limit line length to 70-80 characters for optimal readability

2. **Hierarchy**
   - Use size, weight, and color to establish clear hierarchy
   - Maintain consistent heading levels for proper document structure
   - Use spacing to group related text elements

3. **Responsive Adjustments**
   - Scale text sizes proportionally for different screen sizes
   - Adjust line height for smaller screens
   - Maintain minimum 14px body text even on mobile

### Spacing System

Based on an 8px grid with 4px for fine adjustments:

| Name | Size | Token | Usage |
|------|------|-------|-------|
| Micro | 4px | `--space-micro` | Minimal spacing, tight relationships |
| Small | 8px | `--space-small` | Default spacing between related elements |
| Medium | 16px | `--space-medium` | Standard spacing between distinct elements |
| Large | 24px | `--space-large` | Section separations |
| XLarge | 32px | `--space-xlarge` | Major component separations |
| 2XLarge | 48px | `--space-2xlarge` | Page section separations |
| 3XLarge | 64px | `--space-3xlarge` | Major layout divisions |

#### Spacing Usage Guidelines

1. **Consistent Application**
   - Use spacing tokens instead of arbitrary values
   - Apply consistent spacing within component types
   - Maintain appropriate spacing between different element types

2. **Responsive Adjustments**
   - Reduce spacing proportionally on smaller screens
   - Maintain minimum touch target sizes (44px × 44px)
   - Ensure adequate whitespace even on dense layouts

3. **Hierarchy and Grouping**
   - Use larger spacing to separate distinct sections
   - Use smaller spacing to indicate related elements
   - Create visual rhythm through consistent spacing patterns

### Elevation & Shadows

| Level | Token | CSS | Usage |
|-------|-------|-----|-------|
| Level 0 | `--elevation-0` | `none` | Flat elements, no shadow |
| Level 1 | `--elevation-1` | `0 2px 4px rgba(0,0,0,0.05)` | Cards, subtle elements |
| Level 2 | `--elevation-2` | `0 4px 8px rgba(0,0,0,0.08)` | Floating action buttons, active cards |
| Level 3 | `--elevation-3` | `0 8px 16px rgba(0,0,0,0.1)` | Modals, dialogs, elevated content |
| Level 4 | `--elevation-4` | `0 16px 24px rgba(0,0,0,0.12)` | Important prompts, onboarding elements |

#### Elevation Usage Guidelines

1. **Hierarchy Reinforcement**
   - Use elevation to emphasize interactive and important elements
   - Higher elevation indicates closer proximity to the user
   - Maintain consistent elevation levels for similar element types

2. **Animation Considerations**
   - Animate elevation changes for state transitions
   - Use appropriate easing for natural-feeling transitions
   - Consider subtle color/opacity shifts with elevation changes

3. **Performance Optimization**
   - Use GPU-accelerated properties for shadow animations
   - Consider using SVG filters for complex shadow effects
   - Optimize shadow rendering for performance on mobile devices

### Border Radius

| Name | Size | Token | Usage |
|------|------|-------|-------|
| Small | 4px | `--radius-small` | Buttons, input fields, chips |
| Medium | 8px | `--radius-medium` | Cards, panels, containers |
| Large | 15px | `--radius-large` | Modals, prominent containers |
| XLarge | 20px | `--radius-xlarge` | Floating action buttons, feature callouts |
| Pill | 999px | `--radius-pill` | Tags, status indicators, avatars |

---

## Core Components

### Buttons

Buttons provide clear affordances for user actions and are designed to be easily identifiable and accessible.

#### Button Variants

1. **Primary Button**
   - Purpose: Primary actions, main calls-to-action
   - Appearance: Filled with gradient background
   - States: Default, Hover, Active, Focus, Disabled

2. **Secondary Button**
   - Purpose: Alternative actions, secondary importance
   - Appearance: Outlined with color border
   - States: Default, Hover, Active, Focus, Disabled

3. **Tertiary Button**
   - Purpose: Minor actions, interface controls
   - Appearance: Text-only with optional icon
   - States: Default, Hover, Active, Focus, Disabled

4. **Icon Button**
   - Purpose: Compact controls, toolbar actions
   - Appearance: Icon only, circular background on interaction
   - States: Default, Hover, Active, Focus, Disabled

#### Button Sizes

| Size | Height | Padding | Text Size | Icon Size | Use Case |
|------|--------|---------|-----------|-----------|----------|
| Small | 32px | 12px | 14px | 16px | Dense interfaces, tables |
| Medium | 40px | 16px | 14px | 18px | Most interface contexts |
| Large | 48px | 24px | 16px | 20px | Primary page actions |

#### Implementation Details

```html
<button class="htms-button htms-button--primary htms-button--medium">
  <span class="htms-button__icon">
    <!-- Optional icon -->
  </span>
  <span class="htms-button__text">Button Text</span>
</button>
```

```css
.htms-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: none;
  cursor: pointer;
  font-family: var(--font-family-primary);
  font-weight: var(--font-weight-medium);
  transition: all 200ms ease-out;
  border-radius: var(--radius-small);
}

.htms-button--primary {
  background: var(--gradient-primary);
  color: var(--color-neutral-white);
}

.htms-button--primary:hover {
  box-shadow: 0 2px 8px rgba(61, 90, 254, 0.3);
}

.htms-button--medium {
  height: 40px;
  padding: 0 16px;
  font-size: 14px;
}

.htms-button--disabled {
  opacity: 0.5;
  cursor: not-allowed;
  pointer-events: none;
}
```

#### Accessibility Considerations

- Ensure `button` elements are used for buttons, or `role="button"` for custom elements
- Maintain minimum touch target size of 44px × 44px for mobile
- Include `aria-label` for icon-only buttons
- Visually distinct focus states for keyboard navigation
- Disabled buttons should include `aria-disabled="true"`

### Input Fields

Input fields allow users to enter and edit text with appropriate feedback and guidance.

#### Input Variants

1. **Text Input**
   - Purpose: Single-line text entry
   - Features: Label, placeholder, helper text, character count
   - States: Default, Focus, Hover, Error, Disabled

2. **Textarea**
   - Purpose: Multi-line text entry
   - Features: Auto-expanding height, label, character count
   - States: Default, Focus, Hover, Error, Disabled

3. **Search Input**
   - Purpose: Search functionality
   - Features: Search icon, clear button, autocomplete
   - States: Default, Focus, Hover, Results, Disabled

4. **AI-Enhanced Input**
   - Purpose: Input with intelligent assistance
   - Features: Suggestions, auto-completion, contextual help
   - States: Default, Focus, Suggestions, Processing

#### Input Implementation

```html
<div class="htms-input">
  <label class="htms-input__label" for="username">Username</label>
  <div class="htms-input__container">
    <input 
      id="username" 
      class="htms-input__field" 
      type="text" 
      placeholder="Enter username"
    />
    <div class="htms-input__icon">
      <!-- Optional icon -->
    </div>
  </div>
  <div class="htms-input__helper">Choose a unique username</div>
  <div class="htms-input__error">Username already taken</div>
</div>
```

```css
.htms-input {
  margin-bottom: var(--space-medium);
}

.htms-input__label {
  display: block;
  margin-bottom: var(--space-micro);
  font-size: var(--text-body-2);
  color: var(--color-neutral-dark);
}

.htms-input__container {
  position: relative;
  display: flex;
  align-items: center;
}

.htms-input__field {
  width: 100%;
  height: 40px;
  padding: 0 var(--space-medium);
  border: 1px solid var(--color-neutral-light);
  border-radius: var(--radius-small);
  font-size: var(--text-body-1);
  transition: all 200ms ease-out;
}

.htms-input__field:focus {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(61, 90, 254, 0.1);
  outline: none;
}

.htms-input--error .htms-input__field {
  border-color: var(--color-error);
}

.htms-input__helper,
.htms-input__error {
  margin-top: var(--space-micro);
  font-size: var(--text-caption);
}

.htms-input__helper {
  color: var(--color-neutral-medium);
}

.htms-input__error {
  color: var(--color-error);
}
```

### Cards

Cards are containers that group related content and actions, providing a flexible structure for various content types.

#### Card Variants

1. **Basic Card**
   - Purpose: General content containers
   - Features: Title, content area, optional footer
   - States: Default, Hover, Active, Disabled

2. **Interactive Card**
   - Purpose: Clickable/selectable items
   - Features: Hover effects, selection states
   - States: Default, Hover, Active, Selected, Disabled

3. **Status Card**
   - Purpose: Display item status and metrics
   - Features: Status indicators, progress visualization
   - States: On track, At risk, Blocked, Complete

4. **Action Card**
   - Purpose: Card with primary actions
   - Features: Prominent action buttons/links
   - States: Default, Hover, Active, Disabled

#### Card Implementation

```html
<div class="htms-card htms-card--interactive">
  <div class="htms-card__header">
    <h2 class="htms-card__title">Card Title</h2>
    <div class="htms-card__actions">
      <!-- Optional actions -->
    </div>
  </div>
  <div class="htms-card__content">
    <!-- Card content -->
  </div>
  <div class="htms-card__footer">
    <!-- Optional footer -->
  </div>
</div>
```

```css
.htms-card {
  background-color: var(--color-neutral-white);
  border-radius: var(--radius-medium);
  box-shadow: var(--elevation-1);
  overflow: hidden;
  transition: box-shadow 200ms ease-out;
}

.htms-card--interactive {
  cursor: pointer;
}

.htms-card--interactive:hover {
  box-shadow: var(--elevation-2);
}

.htms-card__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-medium);
  border-bottom: 1px solid var(--color-neutral-lighter);
}

.htms-card__title {
  margin: 0;
  font-size: var(--text-heading-2);
  font-weight: var(--font-weight-semibold);
  color: var(--color-neutral-darker);
}

.htms-card__content {
  padding: var(--space-medium);
}

.htms-card__footer {
  padding: var(--space-medium);
  border-top: 1px solid var(--color-neutral-lighter);
  background-color: var(--color-neutral-lighter);
}
```

### Navigation Components

#### Global Sidebar

```html
<nav class="htms-sidebar">
  <div class="htms-sidebar__logo">
    <!-- Logo -->
  </div>
  <ul class="htms-sidebar__menu">
    <li class="htms-sidebar__item htms-sidebar__item--active">
      <a href="#" class="htms-sidebar__link">
        <span class="htms-sidebar__icon">
          <!-- Icon -->
        </span>
        <span class="htms-sidebar__text">Dashboard</span>
      </a>
    </li>
    <!-- Additional menu items -->
  </ul>
  <div class="htms-sidebar__footer">
    <!-- User profile, settings, etc. -->
  </div>
</nav>
```

#### Top Navigation Bar

```html
<header class="htms-topbar">
  <div class="htms-topbar__start">
    <div class="htms-topbar__ai-indicator">
      <!-- AI status indicator -->
    </div>
    <div class="htms-topbar__breadcrumbs">
      <!-- Breadcrumb navigation -->
    </div>
  </div>
  <div class="htms-topbar__end">
    <div class="htms-topbar__search">
      <!-- Search component -->
    </div>
    <div class="htms-topbar__actions">
      <!-- Notification, messages, etc. -->
    </div>
    <div class="htms-topbar__profile">
      <!-- User profile -->
    </div>
  </div>
</header>
```

### Status and Feedback Components

#### Progress Indicator

```html
<div class="htms-progress">
  <div class="htms-progress__label">Task Progress</div>
  <div class="htms-progress__bar">
    <div class="htms-progress__fill" style="width: 75%"></div>
  </div>
  <div class="htms-progress__text">75% Complete</div>
</div>
```

#### Status Badge

```html
<span class="htms-badge htms-badge--success">Complete</span>
<span class="htms-badge htms-badge--warning">In Progress</span>
<span class="htms-badge htms-badge--error">Blocked</span>
<span class="htms-badge htms-badge--info">New</span>
```

#### Toast Notification

```html
<div class="htms-toast htms-toast--success">
  <div class="htms-toast__icon">
    <!-- Icon -->
  </div>
  <div class="htms-toast__content">
    <div class="htms-toast__title">Success</div>
    <div class="htms-toast__message">Task successfully updated</div>
  </div>
  <button class="htms-toast__close">
    <!-- Close icon -->
  </button>
</div>
```

### AI Components

#### AI Assistant Button

```html
<button class="htms-ai-button">
  <div class="htms-ai-button__indicator"></div>
  <div class="htms-ai-button__pulse"></div>
</button>
```

```css
.htms-ai-button {
  position: fixed;
  bottom: 24px;
  right: 24px;
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background: var(--gradient-secondary);
  border: none;
  cursor: pointer;
  box-shadow: var(--elevation-3);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.htms-ai-button__indicator {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: white;
}

.htms-ai-button__pulse {
  position: absolute;
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background: var(--color-secondary);
  opacity: 0.3;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% {
    transform: scale(1);
    opacity: 0.3;
  }
  50% {
    transform: scale(1.2);
    opacity: 0.15;
  }
  100% {
    transform: scale(1);
    opacity: 0.3;
  }
}
```

#### AI Insight Panel

```html
<div class="htms-insight">
  <div class="htms-insight__header">
    <div class="htms-insight__avatar">
      <!-- AI avatar -->
    </div>
    <div class="htms-insight__title">Resource Allocation Alert</div>
  </div>
  <div class="htms-insight__content">
    Alex is overloaded. Consider reassigning 2 tasks to Jamie who has capacity.
  </div>
  <div class="htms-insight__actions">
    <button class="htms-button htms-button--secondary htms-button--small">
      Dismiss
    </button>
    <button class="htms-button htms-button--primary htms-button--small">
      View Options
    </button>
  </div>
</div>
```

---

## UI Patterns

### Dashboard Layout

The dashboard is designed for at-a-glance information with personalized insights and prioritized tasks.

#### Structure

```html
<div class="htms-dashboard">
  <section class="htms-dashboard__briefing">
    <!-- AI-generated morning briefing -->
  </section>
  
  <section class="htms-dashboard__focus">
    <header class="htms-section-header">
      <h2 class="htms-section-header__title">Today's Focus</h2>
      <div class="htms-section-header__subtitle">
        AI recommended based on priorities and deadlines
      </div>
    </header>
    
    <div class="htms-dashboard__focus-grid">
      <!-- Focus task cards -->
    </div>
  </section>
  
  <div class="htms-dashboard__columns">
    <section class="htms-dashboard__activity">
      <!-- Recent activity feed -->
    </section>
    
    <section class="htms-dashboard__insights">
      <!-- AI insights and recommendations -->
    </section>
  </div>
</div>
```

#### Responsive Behavior

- Desktop: Full three-column layout
- Tablet: Two-column layout with stacked sections
- Mobile: Single column with prioritized content

### Task Detail View

The task detail view provides comprehensive information and actions for individual work items.

#### Structure

```html
<div class="htms-task-detail">
  <header class="htms-task-detail__header">
    <!-- Task title, state, project -->
  </header>
  
  <section class="htms-task-detail__progress">
    <!-- Milestone-based progress visualization -->
  </section>
  
  <div class="htms-task-detail__ai-insight">
    <!-- Contextual AI insight banner -->
  </div>
  
  <div class="htms-task-detail__columns">
    <div class="htms-task-detail__meta">
      <!-- Task metadata, dates, assignees -->
    </div>
    
    <div class="htms-task-detail__subtasks">
      <!-- Subtask management -->
    </div>
    
    <div class="htms-task-detail__activity">
      <!-- DTPE activity tracking -->
    </div>
  </div>
  
  <footer class="htms-task-detail__actions">
    <!-- Primary and secondary actions -->
  </footer>
</div>
```

### Team Space

The team space provides a collaborative environment for team members to coordinate and communicate.

#### Structure

```html
<div class="htms-team-space">
  <header class="htms-team-space__header">
    <!-- Team name, key metrics -->
  </header>
  
  <section class="htms-team-space__members">
    <!-- Team member roster with availability -->
  </section>
  
  <div class="htms-team-space__columns">
    <section class="htms-team-space__priorities">
      <!-- Team priorities and focus -->
    </section>
    
    <section class="htms-team-space__activity">
      <!-- Team activity and discussions -->
    </section>
  </div>
</div>
```

### Notification Center

The notification center provides a centralized view of alerts, updates, and messages.

#### Structure

```html
<div class="htms-notifications">
  <header class="htms-notifications__header">
    <!-- Title, filter options -->
  </header>
  
  <section class="htms-notifications__smart">
    <!-- AI-prioritized notifications -->
  </section>
  
  <section class="htms-notifications__list">
    <!-- Chronological notification list -->
  </section>
</div>
```

## Animation & Motion

### Animation Principles

HTMS animations follow principles that create a cohesive, natural, and purposeful motion language:

1. **Meaningful Motion**
   - Every animation serves a purpose
   - Motion communicates relationships
   - Animations provide feedback
   - Motion guides attention

2. **Natural Physics**
   - Realistic acceleration and deceleration
   - Mass-based timing (smaller elements move faster)
   - Proper momentum and inertia
   - Easing functions that mimic real-world physics

3. **Consistent Language**
   - Similar elements move in similar ways
   - Consistent timing and easing
   - Predictable behavioral patterns
   - Unified motion system

### Easing Functions

| Name | CSS Value | Token | Usage |
|------|-----------|-------|-------|
| Standard | cubic-bezier(0.4, 0.0, 0.2, 1) | `--ease-standard` | Most UI transitions |
| Enter | cubic-bezier(0.0, 0.0, 0.2, 1) | `--ease-enter` | Elements entering the view |
| Exit | cubic-bezier(0.4, 0.0, 1, 1) | `--ease-exit` | Elements leaving the view |
| Emphasize | cubic-bezier(0.0, 0.0, 0.2, 1.4) | `--ease-emphasize` | Attention-drawing animations |

### Duration Guidelines

| Type | Duration | Token | Usage |
|------|----------|-------|-------|
| Instant | 100ms | `--duration-instant` | Immediate feedback (button presses) |
| Quick | 200ms | `--duration-quick` | Simple transitions (hover states) |
| Standard | 300ms | `--duration-standard` | Most interface transitions |
| Complex | 500ms | `--duration-complex` | Multi-stage or emphasized animations |

### Common Animation Patterns

#### State Change Transitions

```css
.htms-button {
  background-color: var(--color-primary);
  transition: 
    background-color var(--duration-quick) var(--ease-standard),
    transform var(--duration-quick) var(--ease-standard),
    box-shadow var(--duration-quick) var(--ease-standard);
}

.htms-button:hover {
  background-color: var(--color-primary-light);
  transform: translateY(-1px);
  box-shadow: var(--elevation-2);
}

.htms-button:active {
  background-color: var(--color-primary-dark);
  transform: translateY(1px);
  box-shadow: var(--elevation-1);
}
```

#### Page Transitions

```css
/* Page entering the view */
.htms-page-enter {
  opacity: 0;
  transform: translateX(20px);
}

.htms-page-enter-active {
  opacity: 1;
  transform: translateX(0);
  transition: 
    opacity var(--duration-standard) var(--ease-enter),
    transform var(--duration-standard) var(--ease-enter);
}

/* Page exiting the view */
.htms-page-exit {
  opacity: 1;
  transform: translateX(0);
}

.htms-page-exit-active {
  opacity: 0;
  transform: translateX(-20px);
  transition: 
    opacity var(--duration-standard) var(--ease-exit),
    transform var(--duration-standard) var(--ease-exit);
}
```

#### Loading States

```css
@keyframes htms-spinner {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.htms-loading-spinner {
  width: 24px;
  height: 24px;
  border: 2px solid var(--color-primary);
  border-top-color: transparent;
  border-radius: 50%;
  animation: htms-spinner 1s linear infinite;
}

@keyframes htms-pulse {
  0% { opacity: 0.6; }
  50% { opacity: 0.3; }
  100% { opacity: 0.6; }
}

.htms-loading-pulse {
  animation: htms-pulse 1.5s ease-in-out infinite;
}
```

#### Content Appearance

```css
@keyframes htms-fade-in-up {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.htms-content-appear {
  animation: htms-fade-in-up var(--duration-standard) var(--ease-enter);
}

/* Staggered content appearance */
.htms-content-stagger > * {
  animation: htms-fade-in-up var(--duration-standard) var(--ease-enter) both;
}

.htms-content-stagger > *:nth-child(1) { animation-delay: 0ms; }
.htms-content-stagger > *:nth-child(2) { animation-delay: 50ms; }
.htms-content-stagger > *:nth-child(3) { animation-delay: 100ms; }
.htms-content-stagger > *:nth-child(4) { animation-delay: 150ms; }
.htms-content-stagger > *:nth-child(5) { animation-delay: 200ms; }
```

### React Implementation

```jsx
import React from 'react';
import { motion } from 'framer-motion';

// Example animation hook for consistent use of animations
export const useHtmsAnimation = () => {
  // Standard variants for common animations
  const fadeIn = {
    hidden: { opacity: 0 },
    visible: { 
      opacity: 1,
      transition: { 
        duration: 0.3, 
        ease: [0.0, 0.0, 0.2, 1] 
      }
    }
  };
  
  const slideInUp = {
    hidden: { opacity: 0, y: 20 },
    visible: { 
      opacity: 1, 
      y: 0,
      transition: { 
        duration: 0.3, 
        ease: [0.0, 0.0, 0.2, 1] 
      }
    }
  };
  
  // Function to create staggered animations for lists
  const createStaggered = (childVariants, staggerDelay = 0.05) => ({
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: {
        when: "beforeChildren",
        staggerChildren: staggerDelay
      }
    },
    childVariants
  });
  
  return {
    fadeIn,
    slideInUp,
    createStaggered
  };
};

// Usage example
const AnimatedCard = () => {
  const { slideInUp } = useHtmsAnimation();
  
  return (
    <motion.div
      initial="hidden"
      animate="visible"
      variants={slideInUp}
      className="htms-card"
    >
      Card content
    </motion.div>
  );
};
```

### Performance Considerations

To ensure smooth animations:

1. **Optimize for Compositor**
   - Prefer `transform` and `opacity` properties
   - Avoid animating layout properties (`width`, `height`, `top`, etc.)
   - Use `will-change` sparingly and strategically

2. **Reduce Repaints**
   - Promote elements to their own layer when appropriate
   - Avoid unnecessary DOM changes during animations
   - Use `transform: translateZ(0)` for hardware acceleration when needed

3. **Test on Target Devices**
   - Verify performance on lower-powered devices
   - Provide reduced motion alternatives
   - Consider conditional animations based on device capability

4. **Respect User Preferences**
   - Honor reduced motion settings
   - Provide non-animated alternatives
   - Test with accessibility tools

```css
/* Honor reduced motion preferences */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.001ms !important;
    transition-duration: 0.001ms !important;
    animation-iteration-count: 1 !important;
  }
  
  /* Provide alternative indicators */
  .htms-loading-spinner {
    animation: none;
    border: 2px solid var(--color-primary);
    border-top-color: var(--color-primary-light);
  }
}
```

---

## Mobile & Responsive Guidelines

### Mobile-First Approach

HTMS design system follows a mobile-first philosophy:

1. **Design for Mobile First**
   - Begin with mobile constraints
   - Focus on core functionality
   - Establish information hierarchy

2. **Progressive Enhancement**
   - Add features for larger screens
   - Enhance interaction patterns
   - Optimize for input methods

3. **Consistent Experience**
   - Core functionality identical across devices
   - Interaction patterns adapted appropriately
   - Brand experience maintained

### Touch Target Guidelines

Ensure all interactive elements are accessible on touch devices:

| Element Type | Minimum Size | Recommended Size | Spacing |
|--------------|--------------|------------------|---------|
| Primary Actions | 48×48px | 56×56px | 8px minimum |
| Secondary Actions | 44×44px | 48×48px | 8px minimum |
| Icon Buttons | 44×44px | 48×48px | 8px minimum |
| Form Controls | 44×44px | 48×48px | 8px minimum |

### Mobile Component Adaptations

#### Cards
- Full-width on mobile
- Simplified content
- Optimized for vertical scrolling
- Touch-friendly actions

#### Navigation
- Bottom navigation bar on mobile
- Collapsible sidebar on tablet
- Full sidebar on desktop
- Context-aware back buttons

#### Forms
- Stacked layout on mobile
- Larger input fields
- Native input types
- Simplified validation

#### Data Visualization
- Simplified charts on mobile
- Interactive exploration on larger screens
- Key metrics emphasized
- Optional detailed views

---

## Dark Mode Support

The HTMS design system includes comprehensive dark mode support with appropriate contrast and readability.

### Dark Mode Color Palette

| Light Mode | Dark Mode | Token | Usage |
|------------|-----------|-------|-------|
| #ffffff | #121212 | `--color-background` | Application background |
| #f5f7fa | #1e1e1e | `--color-surface` | Card and component surfaces |
| #263238 | #eceff1 | `--color-on-surface` | Primary text on surfaces |
| #3d5afe | #738ffe | `--color-primary` | Primary brand color |
| #7c4dff | #b49eff | `--color-secondary` | Secondary brand color |

### Dark Mode Implementation

```css
:root {
  /* Light mode default variables */
  --color-background: #ffffff;
  --color-surface: #f5f7fa;
  --color-on-surface: #263238;
  --color-primary: #3d5afe;
  --color-secondary: #7c4dff;
  /* ... other variables ... */
}

@media (prefers-color-scheme: dark) {
  :root {
    /* Dark mode variables */
    --color-background: #121212;
    --color-surface: #1e1e1e;
    --color-on-surface: #eceff1;
    --color-primary: #738ffe;
    --color-secondary: #b49eff;
    /* ... other variables ... */
  }
}

/* Components use the variables regardless of mode */
.htms-card {
  background-color: var(--color-surface);
  color: var(--color-on-surface);
}
```

### Dark Mode Considerations

1. **Contrast Ratios**
   - Maintain minimum 4.5:1 for normal text
   - Adjust colors to maintain readability
   - Test all components in both modes

2. **Content Hierarchy**
   - Preserve visual hierarchy in dark mode
   - Adjust emphasis techniques appropriately
   - Maintain information priority

3. **Visual Indicators**
   - Ensure states remain distinct in dark mode
   - Adjust shadows and elevation for dark backgrounds
   - Maintain visual cues for interactive elements

---

## Data Visualization Guidelines

### Chart & Graph Standards

HTMS uses a consistent approach to data visualization:

1. **Chart Types**
   - Line charts for trends over time
   - Bar charts for comparison of categories
   - Pie/donut charts for part-to-whole (limited use)
   - Area charts for cumulative values
   - Scatter plots for correlation

2. **Color Usage**
   - Primary data series: Brand colors
   - Secondary data: Neutral colors
   - Alerts/exceptions: Functional colors
   - Limit to 6 colors per visualization

3. **Accessibility**
   - Patterns in addition to colors
   - Clear labels and legends
   - Screen reader annotations
   - Interactive elements meet touch target guidelines

### Data Visualization Components

#### Chart Container

```html
<div class="htms-chart">
  <div class="htms-chart__header">
    <h3 class="htms-chart__title">Project Progress</h3>
    <div class="htms-chart__controls">
      <!-- Optional controls -->
    </div>
  </div>
  <div class="htms-chart__content">
    <!-- Chart content -->
  </div>
  <div class="htms-chart__footer">
    <div class="htms-chart__legend">
      <!-- Legend items -->
    </div>
  </div>
</div>
```

#### Data Table

```html
<table class="htms-data-table">
  <thead>
    <tr>
      <th>Project</th>
      <th>Status</th>
      <th>Progress</th>
      <th>Due Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Design System</td>
      <td>
        <span class="htms-badge htms-badge--success">On Track</span>
      </td>
      <td>
        <div class="htms-progress">
          <div class="htms-progress__bar" style="width: 75%"></div>
        </div>
      </td>
      <td>Mar 15, 2025</td>
    </tr>
    <!-- Additional rows -->
  </tbody>
</table>
```

### Interactive Visualization Guidelines

1. **Tooltips & Hovers**
   - Consistent appearance across charts
   - Appear on hover/focus
   - Include all relevant data points
   - Accessible via keyboard

2. **Filtering & Controls**
   - Consistent control placement
   - Clear visual feedback for selection
   - Maintain context when filtering
   - Reset option always available

3. **Responsive Behavior**
   - Simplified views on smaller screens
   - Maintain data integrity across sizes
   - Touch-friendly interactive elements
   - Optional detailed views for complex data

---

## Icon System

### Icon Design Principles

HTMS uses a consistent icon system following these principles:

1. **Visual Consistency**
   - Uniform line weight (2px standard)
   - Consistent corner radius (2px)
   - Common visual language
   - Balanced optical weight

2. **Semantic Meaning**
   - Clear, recognizable metaphors
   - Consistent use across platform
   - Avoid ambiguous symbols
   - Focus on universal concepts

3. **Technical Standards**
   - SVG format for all icons
   - 24×24px default viewport
   - Optimized file size
   - Responsive scaling

### Icon Categories

| Category | Purpose | Examples |
|----------|---------|----------|
| Navigation | Main navigation areas | Home, Dashboard, Calendar, Team |
| Actions | User operations | Add, Edit, Delete, Share |
| Objects | Content types | Task, Document, Comment, Project |
| Status | State indicators | Complete, Warning, Error, Info |
| AI Features | AI-related functionality | Assistant, Suggestion, Analysis |

### Icon Implementation

Icons can be implemented in several ways:

#### SVG Sprite

```html
<svg class="htms-icon">
  <use xlink:href="#icon-dashboard"></use>
</svg>
```

#### React Component

```jsx
import { Icon } from '@htms/design-system';

// Usage
<Icon name="dashboard" size="medium" color="primary" />
```

#### CSS Background

```css
.dashboard-icon {
  width: 24px;
  height: 24px;
  background-image: url('../icons/dashboard.svg');
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
}
```

### Icon Sizing

| Size | Dimensions | Usage |
|------|------------|-------|
| Small | 16×16px | Dense UIs, table rows |
| Medium | 24×24px | Standard UI, buttons |
| Large | 32×32px | Feature highlights |
| XLarge | 48×48px | Marketing, empty states |

---

## Voice & Tone Guidelines

### Content Principles

The HTMS platform uses a consistent voice and tone:

1. **Clear & Direct**
   - Use simple, straightforward language
   - Avoid jargon and technical terms unless necessary
   - Focus on user goals and actions
   - Be concise and scannable

2. **Helpful & Supportive**
   - Provide guidance without being condescending
   - Focus on solutions, not problems
   - Anticipate user needs
   - Offer next steps

3. **Professional & Approachable**
   - Balance professionalism with friendliness
   - Maintain consistent level of formality
   - Avoid overly casual or overly formal extremes
   - Use natural, conversational language

### UI Text Guidelines

#### Button Labels

- Use action verbs
- Be concise (1-3 words)
- Be specific about the action
- Use sentence case

**Examples:**
- "Create Task" (not "New Task")
- "Send Invitation" (not "Send")
- "Generate Report" (not "Report")

#### Form Labels & Help Text

- Clear, descriptive labels
- Optional help text for complex fields
- Placeholder text as examples, not instructions
- Informative error messages with solutions

**Examples:**
- Label: "Due Date"
- Help text: "When this task needs to be completed"
- Placeholder: "Apr 15, 2025"
- Error: "Please enter a date in the future"

#### Empty States

- Explain what would normally appear
- Provide clear next steps
- Use supportive, encouraging tone
- Include visual element when appropriate

**Example:**
"You don't have any active tasks yet. Create your first task to get started."

#### Notifications & Alerts

- Be specific about what happened
- Explain implications when necessary
- Provide clear next steps if action is needed
- Use appropriate tone for the situation

**Examples:**
- Success: "Task created successfully. It's been added to your dashboard."
- Warning: "This change will affect 3 team members' workloads."
- Error: "Unable to save changes. Please try again or contact support."

### AI Assistant Voice

The AI assistant in HTMS has a specific personality:

- **Helpful but not intrusive**
- **Confident but not presumptuous**
- **Intelligent but approachable**
- **Supportive but respectful of autonomy**

**Examples:**
- Suggestion: "I noticed you've completed several similar tasks recently. Would you like me to create a template based on these patterns?"
- Analysis: "Based on current progress, this project is at risk of missing its deadline. Consider reassigning these 3 tasks or adjusting the timeline."
- Response: "I've found 5 team members with design skills who have availability next week. Would you like to see their profiles?"

---

## Design-to-Development Handoff

### Design Delivery Process

1. **Design System Reference**
   - Link to design system documentation
   - Component usage guidelines
   - Design token reference

2. **Component Specifications**
   - Detailed component behavior
   - Responsive breakpoints
   - State variations
   - Edge cases

3. **Visual Assets**
   - SVG icons and illustrations
   - Image assets (optimized)
   - Animation references

4. **Implementation Notes**
   - Browser/device considerations
   - Performance recommendations
   - Accessibility requirements

### Tools & Resources

#### Design Tools
- Figma for UI design (primary)
- Adobe XD for complex illustrations
- Principle for motion design

#### Handoff Tools
- Figma Inspect for specifications
- Zeplin for design specs and assets
- Abstract for versioning and history

#### Development Resources
- Storybook for component library
- GitHub repositories for code
- NPM packages for design tokens

### Collaboration Workflow

1. **Design Phase**
   - Create designs in Figma
   - Reference design system components
   - Document behavior specifications
   - Internal design review

2. **Handoff**
   - Component specifications documented
   - Interactive prototype shared
   - Design QA completed
   - Kickoff meeting with developers

3. **Development**
   - Regular sync meetings
   - Design review of implementation
   - Iterative adjustments as needed
   - Final approval and sign-off

---

## Release & Maintenance Strategy

### Versioning Guidelines

The HTMS design system follows semantic versioning:

- **Major (X.0.0)**: Breaking changes, significant visual updates
- **Minor (0.X.0)**: New components, non-breaking enhancements
- **Patch (0.0.X)**: Bug fixes, minor visual adjustments, documentation

### Release Process

1. **Planning**
   - Collect requirements and feedback
   - Prioritize changes
   - Create design proposals
   - Get stakeholder approval

2. **Development**
   - Implement changes in design system
   - Update documentation
   - Create migration guides if needed
   - Internal QA and testing

3. **Release**
   - Version bump according to semver
   - Release notes and changelog
   - Developer notification
   - Support materials

4. **Maintenance**
   - Monitor implementation
   - Collect feedback
   - Address issues
   - Plan next iteration

### Deprecation Policy

When components or patterns need to be deprecated:

1. **Announcement**
   - Notify developers of upcoming deprecation
   - Provide timeline (minimum 2 release cycles)
   - Document replacement patterns

2. **Transition Period**
   - Keep deprecated items functional
   - Add visual indicators in documentation
   - Provide migration examples
   - Support teams during transition

3. **Removal**
   - Remove from active documentation
   - Archive examples for reference
   - Redirect documentation links
   - Release major version update

---

## Getting Started Guide

### For Designers

1. **Access Design Files**
   - Request access to Figma workspace
   - Install design system Figma library
   - Review component documentation

2. **Using Components**
   - Use components from the library
   - Avoid detaching instances when possible
   - Follow usage guidelines
   - Document any necessary customizations

3. **Contributing**
   - Identify improvement opportunities
   - Create proposals in the design system Figma file
   - Present in design critique
   - Follow contribution process

### For Developers

1. **Installation**
   ```bash
   # Using npm
   npm install @htms/design-system
   
   # Using yarn
   yarn add @htms/design-system
   ```

2. **Basic Setup**
   ```jsx
   // In your application entry point
   import { ThemeProvider } from '@htms/design-system';
   
   const App = () => (
     <ThemeProvider>
       <YourApplication />
     </ThemeProvider>
   );
   ```

3. **Using Components**
   ```jsx
   import { Button, Card, Input } from '@htms/design-system';
   
   const MyComponent = () => (
     <Card>
       <h2>Create Task</h2>
       <Input label="Task Name" placeholder="Enter task name" />
       <Button variant="primary">Create</Button>
     </Card>
   );
   ```

4. **Documentation Resources**
   - Storybook: [storybook.htms.app](http://storybook.htms.app)
   - GitHub: [github.com/htms/design-system](http://github.com/htms/design-system)
   - NPM: [npmjs.com/package/@htms/design-system](http://npmjs.com/package/@htms/design-system)

### For Product Managers

1. **Understanding the System**
   - Review the design system documentation
   - Understand available components and patterns
   - Learn design principles and guidelines

2. **Planning Features**
   - Reference design system for capabilities
   - Discuss custom needs with design team early
   - Consider accessibility requirements
   - Include design system compatibility in requirements

3. **Feedback and Contributions**
   - Collect user feedback on components
   - Identify improvement opportunities
   - Participate in design system planning
   - Prioritize design system enhancements

---

## Frequently Asked Questions

### General Questions

**Q: What is the HTMS Design System?**
A: The HTMS Design System is a comprehensive set of design standards, components, and guidelines that ensure consistency, quality, and efficiency across the HTMS platform.

**Q: Who maintains the design system?**
A: The design system is maintained by a cross-functional team including designers, developers, and product managers, led by the Design System Lead.

**Q: How often is the design system updated?**
A: The design system follows a regular release cycle with minor updates approximately every 2 weeks and major releases quarterly.

### For Designers

**Q: Can I customize components for specific use cases?**
A: Yes, components can be customized within the parameters defined in the documentation. Significant deviations should be reviewed with the design system team.

**Q: How do I suggest changes to the design system?**
A: Submit proposals through the design system Figma file and present them during the bi-weekly design system review meetings.

**Q: What if I need a component that doesn't exist yet?**
A: First check if existing components can be combined to achieve the needed functionality. If not, document the requirements and submit a component request.

### For Developers

**Q: How do I handle responsive behavior?**
A: Components have built-in responsive behavior. Use the provided responsive utilities and follow the documentation for breakpoint handling.

**Q: Can I use only parts of the design system?**
A: Yes, components are modular and can be imported individually to minimize bundle size.

**Q: How do I debug design system issues?**
A: Check the troubleshooting guide in the documentation, review the GitHub issues, or reach out on the #htms-design-system Slack channel.

### For Product Managers

**Q: How does the design system affect development timelines?**
A: The design system typically accelerates development by providing ready-made, tested components, but may require additional time for custom functionality.

**Q: How do I request new components or patterns?**
A: Submit a component request form detailing the business requirements, use cases, and priority for the new component.

**Q: How do we measure the impact of the design system?**
A: We track metrics including development velocity, design consistency, accessibility compliance, and user satisfaction to measure the design system's effectiveness.
