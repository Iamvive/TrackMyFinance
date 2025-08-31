# Spend-It Module - Revised UX Specification

## Overview
The Spend-It module helps users track their monthly expenses through predefined categories, with dynamic reordering based on spending patterns. Users fill in applicable expenses once and can update them as needed, with automatic recalculation of totals and insights.

## Key Features
- One-time setup with easy monthly updates
- Skip-friendly category system
- Automatic recalculation
- Dynamic category prioritization
- Monthly expense summary

## User Stories

```
As a user,
I want to see all expense categories at once
So that I can quickly identify which ones apply to me

Acceptance Criteria:
- Clear category listing
- Visual indication of filled vs unfilled categories
- Easy skip functionality
- Progress indicator
- Save partial progress
```

```
As a user,
I want the most relevant categories to appear first
So that I can quickly update my major expenses

Acceptance Criteria:
- Categories ordered by spending amount
- Recently updated categories highlighted
- Frequently used categories prioritized
- Option to pin important categories
- Easy access to skipped categories
```

```
As a user,
I want to easily update my existing expense entries
So that I can keep my monthly expenses current

Acceptance Criteria:
- Quick edit functionality
- Previous value display
- Change highlighting
- Batch update option
- Update history
```

## UI Design

### 1. Main Dashboard
```
┌────────────────────────────┐
│ Monthly Expenses Overview  │
│ [Total: $XXXX]            │
│ [Updated: XX categories]   │
└────────────────────────────┘

┌────────────────────────────┐
│ Top Expense Categories     │
│ ● Housing     $XXXX       │
│ ● Transport   $XXXX       │
│ ● Utilities   $XXXX       │
└────────────────────────────┘

┌────────────────────────────┐
│ Quick Actions              │
│ [Update All] [Reset Month] │
└────────────────────────────┘
```

### 2. Category List View
```
┌────────────────────────────┐
│ Filter: [All] [Active] [Skipped]
└────────────────────────────┘

┌────────────────────────────┐
│ 📍 Housing                 │
│ $1500/month               │
│ Last updated: 2 days ago  │
└────────────────────────────┘

┌────────────────────────────┐
│ 🚗 Transport              │
│ $400/month                │
│ Last updated: 5 days ago  │
└────────────────────────────┘

┌────────────────────────────┐
│ ⚡ Utilities              │
│ Skipped                   │
│ Tap to add               │
└────────────────────────────┘
```

### 3. Category Edit Screen
```
┌────────────────────────────┐
│ Housing Expenses           │
│                           │
│ Monthly Amount            │
│ [$1500____________]       │
│                           │
│ Payment Date              │
│ [Select Date ▼]           │
│                           │
│ Recurring? [Yes] [No]     │
│                           │
│ Notes                     │
│ [________________]        │
│                           │
│ [Save] [Skip] [Delete]    │
└────────────────────────────┘
```

## Interaction Patterns

### 1. Category Card Interactions
- Tap to expand/edit
- Long press to pin/unpin
- Swipe right to mark as paid
- Swipe left to skip
- Pull down to refresh all

### 2. Quick Actions
- "Update All" initiates batch update
- "Copy Last Month" for quick setup
- "Reset Month" clears current month
- "Export Summary" generates report

### 3. Visual Indicators
- Color coding for status
  - Green: Updated this month
  - Yellow: Needs update
  - Grey: Skipped
- Progress circle for completeness
- Trending indicators for changes

## Category Management

### Predefined Categories
1. Housing/Rent
2. Utilities
3. Transportation
4. Insurance
5. Groceries
6. Healthcare
7. Education
8. Entertainment
9. Subscriptions
10. Savings
11. Investments
12. Debt Payments
13. Maintenance
14. Personal Care
15. Miscellaneous

### Category Card Structure
```kotlin
data class CategoryCard(
    val id: String,
    val name: String,
    val icon: ImageVector,
    val amount: BigDecimal?,
    val lastUpdated: LocalDateTime?,
    val isSkipped: Boolean,
    val isPinned: Boolean,
    val priority: Int,
    val notes: String?
)
```

## Monthly Workflow

1. **Initial Setup**
   - Show all categories
   - Allow bulk skip/fill
   - Save progress automatically

2. **Regular Updates**
   - Highlight categories needing updates
   - Show previous month's values
   - Quick copy from last month

3. **Month End**
   - Generate monthly summary
   - Reorder categories based on amounts
   - Prepare for next month

## Animation & Transitions

### 1. Category Reordering
- Smooth reordering animation
- Fade in/out for status changes
- Collapse/expand animations

### 2. Update Flows
- Progressive reveal of fields
- Smooth transitions between states
- Loading indicators for calculations

## Success Metrics
1. Completion rate of monthly updates
2. Time to complete all categories
3. Number of skipped categories
4. Update frequency
5. User satisfaction with order

Would you like me to:
1. Create detailed specs for any specific component?
2. Start implementing the UI components?
3. Design the data structure for this approach?
4. Create the category management system?
