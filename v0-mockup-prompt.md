# ComplianceHub DMS – V0 Mockup Requirements & Prototype Brief

## Overview
- **Purpose:** Enterprise document management system for policy/procedure governance with automated workflows, compliance tracking, and audit management
- **Stack:** Next.js 14 App Router, React 18, Tailwind CSS, shadcn/ui, lucide-react icons
- **Layout:** Top header (brand, search, notifications, user menu) + left sidebar navigation with mobile hamburger menu

## Primary Users
- **Compliance Officer** (monitors dashboards, runs reports, manages retention)
- **Document Author** (creates/edits documents, submits for approval)
- **Approver** (reviews and approves/rejects documents)
- **Employee** (reads and acknowledges documents)

## Key Views (Priority Order)
1) Dashboard
2) Documents Library
3) Workflow Designer
4) Approval Queue
5) Compliance Center
6) Analytics & Reports
7) User Management
8) Settings & Integrations

## Functional Requirements

### Global
- Responsive layout with mobile-first sidebar drawer
- Global search in header with type-ahead suggestions
- Notification bell with dropdown showing recent activities
- User avatar menu with role badge and quick actions
- Breadcrumb navigation for deep hierarchies
- "Demo Mode" badge in header

### Dashboard View
- **KPI Cards Row:**
  - Compliance Rate (big %, color-coded progress ring)
  - Pending Approvals (number with urgency badge)
  - Active Documents (count with trend arrow)
  - Overdue Reviews (alert count with warning icon)
- **Quick Actions Grid:** Upload Document, Start Approval, Run Audit Report, Send Reminder
- **Recent Activity Feed:** Timeline with actor, action, document, timestamp
- **Compliance Heatmap:** Department grid with color intensity by compliance %
- **My Tasks Widget:** Personal action items with due dates and priority badges

### Documents Library
- **Top Bar:** 
  - Search with filters dropdown (type, department, status)
  - View toggle (grid/list/tree)
  - "Upload Document" primary button
  - Bulk actions menu (when items selected)
- **Document Grid/List:**
  - Card: Icon by type, title, version badge, status badge, last modified, owner avatar
  - Quick actions: Preview, Download, Share, Start Workflow
  - Multi-select checkboxes for bulk operations
- **Document Preview Drawer:**
  - Tabs: Preview, Details, Versions, Audit Trail, Acknowledgments
  - Header actions: Download, Edit, Submit for Approval
  - Version comparison slider
  - Acknowledgment progress bar with user list
- **Upload Wizard (Modal):**
  - Step 1: Drag-drop zone with file validation feedback
  - Step 2: Metadata form (title, category, department, tags)
  - Step 3: Workflow selection with visual preview
  - Step 4: Review & Submit with success animation

### Workflow Designer
- **Canvas Area:**
  - Drag-drop workflow builder with connecting lines
  - Node types: Start, Approval, Parallel, Condition, End
  - Each node shows: Label, Assignee avatar, SLA badge
  - Zoom controls and grid toggle
- **Left Panel:** 
  - Node palette with drag handles
  - Saved templates list
  - Workflow properties form
- **Right Panel:**
  - Selected node configuration
  - Assignee picker with role filter
  - SLA settings with business hours toggle
  - Conditional rules builder
- **Top Actions:** Save, Test Run, Activate, Share

### Approval Queue
- **Filter Bar:**
  - Quick filters: My Approvals, Delegated, Overdue, All
  - Search by document name or submitter
  - Sort options: Due Date, Priority, Age
- **Approval Cards:**
  - Document title with type icon
  - Submitter info with avatar
  - Time in queue with SLA indicator
  - Priority badge (High/Medium/Low)
  - Quick approve/reject buttons
  - "Review" opens side panel
- **Review Panel:**
  - Document viewer with annotation tools
  - Previous comments thread
  - Approval form with required comment for rejection
  - "Request Changes" option with specific feedback
  - Digital signature pad component

### Compliance Center
- **Compliance Dashboard:**
  - Organization-wide compliance gauge (0-100%)
  - Department comparison bar chart
  - Overdue acknowledgments table with send reminder actions
  - Policy review calendar with upcoming due dates
- **Acknowledgment Tracking:**
  - Document selector with acknowledgment requirements
  - User progress table: Name, Department, Status (badge), Date, Certificate link
  - Bulk reminder composer with template selection
  - Export compliance report button
- **Audit Packages:**
  - Pre-configured packages list (ISO, GDPR, HIPAA)
  - "Generate Package" wizard with framework selection
  - Package includes checklist with document links
  - One-click ZIP download with folder structure preview
- **Retention Manager:**
  - Documents approaching retention grid
  - Retention policy rules table with edit actions
  - Disposal queue with approval workflow
  - Legal hold toggle with reason field

### Analytics & Reports
- **KPI Overview:**
  - Trend charts: Approval Time, Compliance Rate, Document Growth
  - Top performers leaderboard
  - Bottleneck identification with drill-down
- **Custom Report Builder:**
  - Drag-drop metrics and dimensions
  - Date range picker with presets
  - Visualization type selector
  - Schedule & Subscribe options
- **Saved Reports Gallery:**
  - Report cards with thumbnail previews
  - Last run time and next schedule
  - Quick actions: Run Now, Edit, Share, Export

### User Management
- **Users Table:**
  - Avatar, Name, Email, Role (badge), Department, Status, Last Active
  - Inline edit for role changes
  - Bulk actions: Activate, Deactivate, Reset Password
  - "Add User" opens wizard
- **User Detail Drawer:**
  - Profile tab with all fields
  - Permissions matrix with toggles
  - Document access log
  - Training/acknowledgment history
- **Roles & Permissions:**
  - Role cards with member count
  - Permission matrix grid with module/action intersections
  - "Create Role" with permission template selection

### Settings & Integrations
- **Integration Cards:**
  - Adobe Sign: Status indicator, "Configure" button, last sync time
  - Microsoft 365: Connected badge, permission scopes list
  - Email Gateway: SMTP settings form, "Send Test" button
- **System Settings Tabs:**
  - General: Org info, logo upload, timezone
  - Security: Password policy, session timeout, MFA toggle
  - Notifications: Email templates editor with variables
  - Branding: Color scheme, custom CSS field

## Data Model (Mock)

### Document
```javascript
{
  id, documentNumber, title, type, category, department,
  currentVersion, status, owner, created, modified,
  retentionDate, requiresAcknowledgment, tags[],
  versions[{ number, created, author, changeNote }]
}
```

### Workflow
```javascript
{
  id, name, type, steps[{
    order, type, approver, slaHours, 
    conditions, parallelTo[]
  }], isDefault, isActive
}
```

### Approval
```javascript
{
  id, documentId, workflowId, currentStep,
  status, submitter, submitted, dueDate,
  history[{ step, approver, action, comments, timestamp }]
}
```

### User
```javascript
{
  id, name, email, avatar, role, department,
  permissions[], lastActive, complianceRate
}
```

## Non-Functional Requirements

### Visual Design
- Clean enterprise aesthetic with subtle shadows
- Status badges: Draft (gray), In Review (blue), Approved (green), Published (teal), Archived (neutral)
- Priority badges: High (red), Medium (yellow), Low (gray)
- Consistent icon usage from lucide-react
- Loading skeletons for data fetching simulation
- Success/error toasts for all actions

### Interactions
- Hover states on all interactive elements
- Smooth transitions (150ms) for state changes
- Optimistic UI updates with rollback on "error"
- Command palette (Cmd+K) for quick navigation
- Keyboard shortcuts for common actions
- Drag-drop for file upload and workflow building

### Mobile Experience
- Bottom tab bar for main navigation
- Swipe gestures for approve/reject
- Simplified dashboard with collapsible sections
- Full-screen document viewer with pinch zoom

## State Management
- `currentView`: Active navigation item
- `selectedDocument`: Current document in preview
- `activeFilters`: Applied filters per view
- `workflowBuilder`: Nodes and connections
- `notificationCount`: Unread notifications
- `userPreferences`: Theme, layout, shortcuts

## v0.app Prompt

```
Build a sophisticated mock-only Next.js 14 + Tailwind + shadcn/ui enterprise document management system called "ComplianceHub DMS". Create a fully interactive prototype that DEMONSTRATES the complete user experience without real backends. All data is mocked, all actions show realistic feedback through toasts/modals/animations.

Core Layout:
- Top header: Left (logo + "ComplianceHub DMS" brand), Center (global search with cmd+k), Right (notification bell with count, user avatar with role badge, settings gear)
- Left sidebar (collapsible on mobile): Navigation items with icons - Dashboard, Documents, Workflows, Approvals, Compliance, Analytics, Users, Settings
- "Demo Mode" and "Version 2.0" badges in header
- Dark mode toggle in user menu

Dashboard:
- 4 KPI cards in a row: Compliance Rate (with circular progress), Pending Approvals (with urgency indicator), Active Documents, Overdue Reviews (with warning styling)
- Quick Actions grid: 4 large buttons with icons (Upload, New Workflow, Audit Report, Send Reminders)
- Recent Activity timeline with user avatars and action descriptions
- Department compliance heatmap grid with color coding

Documents Library:
- Top toolbar: Search bar, filter chips (Type, Status, Department), view toggle (grid/list), "Upload Document" primary button
- Document cards in grid: File type icon, title, version badge, status badge, modified date, owner avatar, hover actions (preview, download, workflow)
- Click opens drawer with tabs: Preview (mock PDF viewer), Details (all metadata), Versions (comparison slider), Audit Trail (timeline), Acknowledgments (progress + user list)
- Upload shows multi-step wizard with drag-drop, metadata form, workflow selection

Workflow Designer:
- Visual canvas with drag-drop nodes (Start, Approval, Condition, End)
- Left panel: node palette, saved templates
- Right panel: node configuration when selected
- Connection lines with animation showing flow direction
- Test Run button shows simulated execution with step highlighting

Approval Queue:
- Filter pills: My Tasks, Delegated, Overdue
- Approval cards: Document info, submitter, time waiting, SLA indicator, quick approve/reject buttons
- Review opens side panel with document viewer, comments thread, approval form with digital signature component

Compliance Center:
- Compliance gauge (0-100% animated)
- Acknowledgment tracking table with status badges and reminder actions
- Audit package generator with framework selection (ISO, GDPR, HIPAA)
- Retention manager with documents approaching retention dates

Analytics:
- Interactive charts using mock data: Approval Time Trend, Compliance by Department, Document Growth
- Report builder with drag-drop metrics
- Saved reports gallery with thumbnails

Mock Behaviors:
- All buttons show appropriate toasts ("Document uploaded successfully", "Workflow saved", "Approval submitted")
- Forms validate with error states but don't persist
- Searches filter mock data in real-time
- Modals and drawers animate smoothly
- Loading states show skeletons
- Success actions show confetti or checkmark animations
- Add a "Reset Demo" button in settings to restore initial state

Sample Data:
- 20+ documents across various types (Policy, Procedure, Form, Guide)
- 5+ departments with different compliance levels
- 10+ users with different roles
- Multiple workflows in different states
- Recent activities and notifications

Make it feel production-ready with polished interactions, proper loading states, and thoughtful empty states. Focus on demonstrating the complete user journey from document upload through approval to compliance tracking.
```

## Acceptance Criteria
- All 8 main views accessible and functional with mock data
- Document upload wizard completes with success feedback
- Workflow designer allows node creation and connection
- Approval actions show toast confirmations
- Compliance dashboard displays realistic metrics
- Analytics charts are interactive with hover states
- Mobile responsive with appropriate touch interactions
- Search and filters work across all data tables
- User can complete full journey: upload → workflow → approval → compliance

## Out of Scope (Prototype)
- Real authentication or user sessions
- Actual file upload/storage
- Backend API calls
- Data persistence between sessions
- Real PDF rendering (use placeholder)
- Actual email sending
- Third-party integrations

## Implementation Notes
- Use `setTimeout` to simulate async operations
- Store mock data in context/state for session persistence
- Use Faker.js or similar for realistic mock data
- Implement optimistic updates with simulated rollback
- Add subtle animations for premium feel
- Include helpful empty states with CTAs
- Show progress indicators for multi-step processes
- Use toasts for success/error feedback