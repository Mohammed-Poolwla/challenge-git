# Meraki Practice Management Dashboard - Phase 1 Detailed Wireframes

Source: Meraki Finserve LLC RFP, "Internal Practice Management & Workflow Dashboard - Phase 1 (MVP)"

## 1. Wireframe goals

These low-fidelity wireframes translate the Phase 1 RFP requirements into screen-level product structure for a responsive internal web application. They focus on layout, information hierarchy, field placement, user actions, state changes, and workflow coverage rather than final visual design.

Phase 1 must help Meraki replace spreadsheet-driven operations with one secure system for:

- Client and entity management.
- Proposal-to-engagement conversion.
- Engagement and task creation.
- Configurable task workflows.
- Review points, checklists, sign-off, and carry-forward items.
- Recurring task and year-on-year rollover handling.
- Staff assignment, reassignment, and deactivation.
- Time tracking.
- Notifications.
- Management and staff dashboards.
- Reporting and exports.
- Master data and administrative configuration.

## 2. Wireframe notation

```text
[Field]              Editable input
{Dropdown}           Select list
( )                  Radio button
[ ]                  Checkbox
<Button>             Primary or secondary action
| Tab |              Tab navigation
*                    Required field
!                    Warning, exception, or overdue state
```

All screens assume:

- Authenticated internal users only.
- Role-based visibility.
- US-hosted data storage.
- Tenant-aware data model from day one, even though Phase 1 is internal-only.
- Immutable audit history for sensitive data and important workflow actions.

## 3. Primary roles

| Role | Phase 1 needs |
| --- | --- |
| Administrator | Configure master data, users, roles, formulas, due date rules, workflows, and templates. View all clients, work, time, productivity, audit logs, and reports. |
| Partner / Manager | View firm-wide or team workload, assign staff, review work, approve engagement/task changes, manage bottlenecks, export reports. |
| Reviewer | See tasks assigned for review, raise review points, sign off checklist/review items, move workflow stages. |
| Preparer / Staff | View assigned work, update task progress, respond to review points, complete checklist items, log time. |

## 4. Global information architecture

```text
+--------------------------------------------------------------------------------+
| MERAKI PRACTICE MANAGEMENT                                                     |
| [Search clients, entities, tasks, engagement letters...]   [Alerts] [Profile]  |
+--------------------------------------------------------------------------------+
| Side navigation                         | Main workspace                        |
|-----------------------------------------|---------------------------------------|
| Dashboard                               |                                       |
| Clients                                 | Contextual page content               |
| Entities                                |                                       |
| Proposals & Engagement Letters          |                                       |
| Engagements                             |                                       |
| Tasks                                   |                                       |
| Time                                    |                                       |
| Staff                                   |                                       |
| Reports                                 |                                       |
| Admin Configuration                     |                                       |
| Audit Log                               |                                       |
+--------------------------------------------------------------------------------+
```

### Global header requirements

- Global search must return clients, entities, tasks, proposals, engagements, staff, and document references.
- Notification icon shows unread assignment, due date, and workflow movement alerts.
- Profile menu includes "My settings", "Notification preferences", and "Sign out".
- Role determines navigation items and data shown.
- Every page has a tenant context internally, hidden for Phase 1 unless support/admin roles need to inspect it later.

### Global interaction patterns

- Lists use saved filters, column configuration, sorting, pagination, and export when permitted.
- Detail pages use tabs so users can move between overview, tasks, notes, time, documents, and audit trail without leaving context.
- Mutating actions create an activity log record.
- Sensitive fields are masked by default and require explicit permission to reveal.

## 5. Phase 1 workflow map

```text
Prospect / Client
      |
      v
Client record + entities
      |
      v
Proposal prepared
      |
      v
Accepted proposal -> engagement letter
      |
      v
Preview generated tasks from service templates
      |
      v
Confirm engagement
      |
      v
Tasks created with due dates, staff, workflow, checklists, review points
      |
      v
Preparation -> Review -> Client waiting if applicable -> Completion
      |
      v
Time, sign-off, audit history, reporting
      |
      v
Rollover / recurring generation for future periods
```

## 6. Screen W01 - Secure sign in

```text
+--------------------------------------------------------------------------------+
| MERAKI FINserve                                                                |
| Practice Management Dashboard                                                  |
+--------------------------------------------------------------------------------+
|                                                                                |
|                         Sign in                                                |
|       ------------------------------------------------                         |
|       [Email address *                                      ]                  |
|       [Password *                                           ]                  |
|       [ ] Remember this device                                                 |
|                                                                                |
|       <Sign in>                                                                |
|                                                                                |
|       Forgot password?                                                         |
|                                                                                |
|       Security note: Access is restricted to authorized Meraki users.           |
|       Client data is hosted in US regions and audited.                          |
|                                                                                |
+--------------------------------------------------------------------------------+
```

### Content and behavior

- Show generic error text: "Invalid email or password".
- If MFA is added later, the second step should appear in the same centered card.
- Sign-in attempts should be logged for audit/security review.

## 7. Screen W02 - Administrator dashboard

```text
+--------------------------------------------------------------------------------+
| Dashboard                                      [Date range: This month v]       |
+--------------------------------------------------------------------------------+
| KPI cards                                                                      |
| +--------------+ +--------------+ +--------------+ +--------------+             |
| | Open tasks   | | Overdue      | | Pending rev. | | Hours logged |             |
| | 184          | | ! 23         | | 41           | | 1,246.5      |             |
| +--------------+ +--------------+ +--------------+ +--------------+             |
|                                                                                |
| +---------------------------------------+ +------------------------------------+ |
| | Workload by staff                     | | Work by workflow stage            | |
| | Staff        Open  Overdue  Capacity  | | Awaiting Info    [#####] 28       | |
| | A. Patel      24     3      82%       | | Preparation      [########] 47    | |
| | R. Shah       18     1      70%       | | First Review     [######] 31      | |
| | N. Mehta      36     8      110% !    | | Waiting Client   [###] 15         | |
| | <View staff report>                   | | <View workflow report>            | |
| +---------------------------------------+ +------------------------------------+ |
|                                                                                |
| +---------------------------------------+ +------------------------------------+ |
| | Due soon / overdue                    | | Recent activity                    | |
| | Client  Task   Due     Owner Status   | | 10:42 Task moved to Review        | |
| | ABC     1120S  Jul 25  R.S. !Overdue  | | 10:15 Review point added          | |
| | XYZ     Payroll Jul 26 A.P. Due soon  | | 09:54 Staff reassignment complete | |
| | <Open task command center>            | | <Open audit log>                  | |
| +---------------------------------------+ +------------------------------------+ |
+--------------------------------------------------------------------------------+
```

### Key requirements covered

- Firm-wide visibility for management.
- Overdue work, pending reviews, workload by staff, workflow bottlenecks, and activity.
- Waiting on Client is separate from workflow stage metrics.
- Date range filter controls dashboard widgets.
- Administrator can drill into report screens.

### Filters

- Date range.
- Service type.
- Staff member.
- Client group.
- Priority.
- Workflow stage.
- Waiting on Client flag.

## 8. Screen W03 - Staff personal dashboard

```text
+--------------------------------------------------------------------------------+
| My Dashboard                                  [Today v]                         |
+--------------------------------------------------------------------------------+
| +--------------+ +--------------+ +--------------+ +--------------+             |
| | My open      | | Due today    | | Review pts   | | Time today   |             |
| | 18           | | 4            | | 7            | | 5.25 hrs     |             |
| +--------------+ +--------------+ +--------------+ +--------------+             |
|                                                                                |
| My priority work                                                               |
| +------------------------------------------------------------------------------+
| | Priority | Task / Client       | Role      | Due date | Stage       | Action  |
| | High     | 1040 - Smith        | Preparer  | Today    | Preparation | <Open>  |
| | High     | Bookkeeping - ABC   | Reviewer  | Jul 25   | First Review| <Open>  |
| | Medium   | Payroll - XYZ       | Resp.     | Jul 27   | Wait Client | <Open>  |
| +------------------------------------------------------------------------------+
|                                                                                |
| +---------------------------------------+ +------------------------------------+ |
| | My review points                      | | Quick time entry                  | |
| | Task        Item         Status       | | [Task search] [Hours] [Notes]     | |
| | Smith 1040  Missing W-2  Open         | | <Start timer> <Log time>          | |
| | ABC Books   Bank query   Responded    | |                                    | |
| +---------------------------------------+ +------------------------------------+ |
+--------------------------------------------------------------------------------+
```

### Key requirements covered

- Staff dashboard limited to assigned work and personal time logging.
- Tasks surface when assigned as preparer, reviewer, partner, or responsible person.
- Quick path to time entry.
- Review point visibility at item level.

## 9. Screen W04 - Client directory

```text
+--------------------------------------------------------------------------------+
| Clients                                            <Add client>                 |
+--------------------------------------------------------------------------------+
| [Search client name, code, contact, EIN...]                                    |
| {Status: All v} {Group: All v} {Service: All v} {Owner: All v} <Filters>       |
|                                                                                |
| +------------------------------------------------------------------------------+
| | Code     | Client / Group       | Entities | Services | Status | Owner | ... |
| | MF-2026A | Acme Holdings        | 3        | 5        | Active | A.R.  | >   |
| | MF-2026B | Bright Dental Group  | 2        | 2        | Active | R.S.  | >   |
| | MF-2025C | Carter, John         | 1        | 1        | Active | N.M.  | >   |
| +------------------------------------------------------------------------------+
|                                                                                |
| Bulk actions: {Assign owner v} {Change status v} <Export>                      |
+--------------------------------------------------------------------------------+
```

### Add client drawer

```text
+----------------------------------------------------------+
| Add client                                               |
|----------------------------------------------------------|
| [Client legal name *                                  ]  |
| [Display name                                       ]    |
| {Client type *: Individual / Business / Group v}         |
| [Group name if applicable                            ]   |
| [Primary contact name                              ]     |
| [Primary contact email                             ]     |
| [Phone                                             ]     |
| [Address                                           ]     |
|                                                          |
| Client code                                            |
| [Auto-generated after save: formula preview]             |
| Formula: first letters of client name + onboarding year  |
|                                                          |
| {Status: Prospect / Active / Inactive v}                 |
| {Responsible partner * v}                                |
| [Notes                                                ]  |
|                                                          |
| <Cancel> <Save client>                                  |
+----------------------------------------------------------+
```

### Key requirements covered

- Centralized client database.
- Grouping.
- Search, filtering, status tracking.
- System-generated client code with formula preview.

## 10. Screen W05 - Client detail

```text
+--------------------------------------------------------------------------------+
| Client: Acme Holdings                                  [Active] [Code MF-2026A] |
| Responsible partner: A. Runwal                         <Edit> <Add entity>     |
+--------------------------------------------------------------------------------+
| | Overview | Entities | Proposals | Engagements | Tasks | Time | Documents |   |
| | Activity & Audit |                                                           |
+--------------------------------------------------------------------------------+
| Overview                                                                       |
| +---------------------------------------+ +------------------------------------+ |
| | Client information                    | | Operational summary               | |
| | Legal name: Acme Holdings             | | Open engagements: 4               | |
| | Group: Acme Group                     | | Open tasks: 28                    | |
| | Type: Business group                  | | Overdue tasks: ! 3                | |
| | Onboarded: Jan 12, 2026               | | Hours this month: 142.25          | |
| | Primary contact: Maya Lee             | | Last activity: Jul 24, 2026       | |
| +---------------------------------------+ +------------------------------------+ |
|                                                                                |
| Sensitive data section                                                         |
| +------------------------------------------------------------------------------+
| | EIN / SSN: *********        <Reveal if permitted>                            |
| | Access and changes are logged.                                               |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### Entities tab

```text
+------------------------------------------------------------------------------+
| Entities                                                       <Add entity>   |
| Code     | Entity name       | Type      | Services | Open tasks | Status | > |
| AC-001   | Acme LLC          | LLC       | 4        | 18         | Active | > |
| AC-002   | Acme Payroll Inc. | Corp      | 1        | 6          | Active | > |
+------------------------------------------------------------------------------+
```

### Activity and audit tab

```text
+------------------------------------------------------------------------------+
| Activity & Audit                                    {Event type: All v}        |
| Time              User        Event                                              |
| Jul 24 10:42      R. Shah     Task AC-1040 moved First Review -> Preparation    |
| Jul 24 10:12      A. Patel    Client contact updated                           |
| Jul 23 17:20      System      Client code generated: MF-2026A                  |
+------------------------------------------------------------------------------+
```

### Key requirements covered

- Multiple entities under one client/group.
- Engagements managed separately for each entity.
- Sensitive data structure ready, with masked display and audit.
- Client-wise workload and history.

## 11. Screen W06 - Entity detail

```text
+--------------------------------------------------------------------------------+
| Entity: Acme LLC                                      Parent client: Acme       |
| Type: LLC | Status: Active                            <Edit entity>            |
+--------------------------------------------------------------------------------+
| | Overview | Engagements | Tasks | Due Dates | Documents | Activity |          |
+--------------------------------------------------------------------------------+
| +---------------------------------------+ +------------------------------------+ |
| | Entity profile                        | | Service coverage                  | |
| | Legal name: Acme LLC                  | | Bookkeeping       Active          | |
| | Tax classification: Partnership       | | 1065 Tax Prep     Active          | |
| | Federal ID: *********                 | | Payroll           Draft           | |
| | State registrations: CA, NY           | | Advisory          Not active      | |
| +---------------------------------------+ +------------------------------------+ |
|                                                                                |
| Upcoming statutory due dates                                                   |
| +------------------------------------------------------------------------------+
| | Task type | Period | Original due | Extended due | Rule source | Override    |
| | 1065      | 2026   | Mar 15, 2027 | Sep 15, 2027 | IRS rule    | <Request>   |
| | Payroll   | Q3     | Oct 31, 2026 | -            | IRS rule    | <Request>   |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### Key requirements covered

- Entity-level engagement management.
- Due date automation visibility.
- Sensitive identifiers masked and auditable.

## 12. Screen W07 - Proposal workspace

```text
+--------------------------------------------------------------------------------+
| Proposals & Engagement Letters                         <Create proposal>       |
+--------------------------------------------------------------------------------+
| [Search proposal/client/entity] {Status: All v} {Service: All v} <Filters>     |
|                                                                                |
| +------------------------------------------------------------------------------+
| | Proposal # | Client / Entity | Services       | Status     | Amount | Action |
| | P-2026-014 | Acme LLC        | 1065, Advisory | Draft      | $--    | Open   |
| | P-2026-012 | Bright Dental   | Payroll        | Accepted   | $--    | Convert|
| | P-2026-008 | Carter, John    | 1040           | Sent       | $--    | Open   |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### Create proposal screen

```text
+--------------------------------------------------------------------------------+
| Create proposal                                       <Save draft> <Preview>    |
+--------------------------------------------------------------------------------+
| Step 1: Client and entity                                                     |
| [Client search *                                      ] <New client>           |
| [Entity search / create entity                         ]                       |
| {Proposal status: Draft v}                                                     |
|                                                                                |
| Step 2: Services                                                               |
| +------------------------------------------------------------------------------+
| | [ ] Bookkeeping        {Frequency: Monthly v} {Start period: Jul 2026 v}     |
| | [ ] Tax preparation    {Task type: 1120-S v} {Tax year: 2026 v}              |
| | [ ] Payroll            {Frequency: Bi-weekly v}                              |
| | [ ] CFO / Advisory     {Cadence: Monthly v}                                  |
| | [ ] International Tax  {Sub-service v}                                       |
| +------------------------------------------------------------------------------+
|                                                                                |
| Step 3: Staffing defaults                                                      |
| {Preparer v} {Reviewer v} {Partner v} {Responsible person v}                   |
|                                                                                |
| Step 4: Engagement terms                                                       |
| [Effective date *] [Original due date if known] [Priority v]                   |
| [Internal notes                                                           ]    |
|                                                                                |
| Service codes                                                                  |
| Bookkeeping: [Auto-generated preview: BK-AC-2026]                              |
| Tax prep:    [Auto-generated preview: TX-AC-2026]                              |
+--------------------------------------------------------------------------------+
```

### Key requirements covered

- Proposal is prepared for prospect/client.
- Multiple service selections are supported.
- Service codes are formula-driven.
- Staffing defaults flow into generated tasks.

## 13. Screen W08 - Accepted proposal to engagement confirmation

```text
+--------------------------------------------------------------------------------+
| Convert accepted proposal P-2026-012                         <Cancel> <Confirm>|
+--------------------------------------------------------------------------------+
| Proposal summary                                                               |
| Client: Bright Dental Group                                                    |
| Entity: Bright Dental PLLC                                                     |
| Accepted date: [Jul 24, 2026 *]                                                 |
| Engagement letter status: [Accepted]                                            |
|                                                                                |
| Generated engagement preview                                                   |
| +---------------------------------------+ +------------------------------------+ |
| | Engagement details                    | | Default assignments               | |
| | Services: Payroll, Bookkeeping        | | Preparer: A. Patel                | |
| | Start period: Jul 2026                | | Reviewer: R. Shah                 | |
| | Priority: Medium                      | | Partner: A. Runwal                | |
| | Service codes: PR-BD-2026, BK-BD-2026 | | Responsible: N. Mehta             | |
| +---------------------------------------+ +------------------------------------+ |
|                                                                                |
| Task set preview                                                               |
| +------------------------------------------------------------------------------+
| | Create | Task name             | Period | Due date | Est hrs | Checklist |   |
| | [x]    | Payroll processing    | Jul 26 | Jul 31   | 3.0     | Payroll   |   |
| | [x]    | Monthly bookkeeping   | Jul 26 | Aug 15   | 8.0     | Books     |   |
| | [ ]    | Advisory call prep    | Jul 26 | Aug 20   | 2.0     | Advisory  |   |
| +------------------------------------------------------------------------------+
|                                                                                |
| Warnings                                                                       |
| ! Due date rule missing for "Advisory call prep". Select manual due date.      |
|                                                                                |
| <Back to proposal> <Generate engagement and selected tasks>                    |
+--------------------------------------------------------------------------------+
```

### Confirmation behavior

- User must preview tasks before engagement confirmation.
- User may exclude optional tasks before generation.
- Missing template data is shown before confirmation.
- Generated records include activity log entries.
- Engagement letter acceptance produces engagement and corresponding task set.

## 14. Screen W09 - Engagement detail

```text
+--------------------------------------------------------------------------------+
| Engagement: Payroll - Bright Dental PLLC         [Active] [Code PR-BD-2026]    |
| Client: Bright Dental Group | Entity: Bright Dental PLLC                       |
| <Edit> <Add task> <Rollover / recur>                                           |
+--------------------------------------------------------------------------------+
| | Overview | Tasks | Staffing | Letter | Time | Activity & Audit |             |
+--------------------------------------------------------------------------------+
| +---------------------------------------+ +------------------------------------+ |
| | Engagement information                | | Status summary                    | |
| | Service: Payroll                      | | Open tasks: 6                     | |
| | Sub-service: Bi-weekly payroll        | | Overdue: ! 1                      | |
| | Period start: Jul 2026                | | Waiting on client: 2              | |
| | Priority: Medium                      | | Completed this period: 4          | |
| | Billing / budget ref: Optional        | | Actual / estimate: 28 / 32 hrs    | |
| +---------------------------------------+ +------------------------------------+ |
|                                                                                |
| Task list                                                                      |
| +------------------------------------------------------------------------------+
| | Task | Period | Stage | Waiting Client | Preparer | Reviewer | Due | Action  |
| | PR1  | Jul 26 | Preparation | No       | A.P.     | R.S.     | Jul 31 | Open |
| | PR2  | Jul 26 | First Review| Yes      | A.P.     | R.S.     | Jul 31 | Open |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### Key requirements covered

- One client can have multiple engagements.
- Each engagement captures service type, responsible staff, due dates, status, priority, and related tasks.
- Time and budgeted-versus-actual hours visible at engagement level.

## 15. Screen W10 - Task command center

```text
+--------------------------------------------------------------------------------+
| Tasks                                                <Create manual task>       |
+--------------------------------------------------------------------------------+
| [Search task, client, entity, assignee...]                                      |
| {Saved view: My open tasks v} {Service v} {Stage v} {Due date v} {Priority v}  |
| [ ] Show overdue only   [ ] Show waiting on client   <Apply> <Save view>       |
|                                                                                |
| View: ( ) Table  ( ) Board  ( ) Calendar                                       |
|                                                                                |
| Table view                                                                      |
| +------------------------------------------------------------------------------+
| | ! | Task | Client / Entity | Type | Period | Stage | Wait? | Due | Owner | > |
| | ! | 1120S tax prep | Acme LLC | 1120-S | 2026 | First Review | No | Jul25| NM| |
| |   | Monthly books  | Bright   | Books  | Jul  | Awaiting Info| Yes| Aug15| AP| |
| +------------------------------------------------------------------------------+
|                                                                                |
| Bulk actions: {Assign v} {Change stage v} {Set priority v} <Export>           |
+--------------------------------------------------------------------------------+
```

### Board view

```text
+--------------------------------------------------------------------------------+
| Awaiting Info       | Preparation        | First Review       | Completion      |
|---------------------|--------------------|--------------------|-----------------|
| Task card           | Task card          | ! Task card        | Task card       |
| Client / Entity     | Client / Entity    | Client / Entity    | Client / Entity |
| Due date            | Due date           | Due date           | Due date        |
| [Waiting client]    | [High]             | [Overdue]          | [Ready]         |
+--------------------------------------------------------------------------------+
```

### Manual task drawer

```text
+----------------------------------------------------------+
| Create manual task                                       |
|----------------------------------------------------------|
| [Client *] [Entity *] [Engagement optional]              |
| {Task type * v} {Service * v}                            |
| [Period / tax year *]                                    |
| {Difficulty rating v} [Estimated hours]                  |
| {Priority v}                                             |
| {Preparer v} {Reviewer v} {Partner v} {Responsible v}    |
| [Original due date] [Extended due date]                  |
| [ ] Calculate statutory due date from rule               |
| {Workflow template v} {Checklist template v}             |
| [Initial notes]                                          |
| <Cancel> <Create task>                                   |
+----------------------------------------------------------+
```

### Key requirements covered

- Quick identification of pending, overdue, completed, and in-progress work.
- Table, board, and calendar representations of the same task set.
- Manual task creation is supported.
- Bulk assignment and stage changes respect role permissions and audit logging.

## 16. Screen W11 - Task detail

```text
+--------------------------------------------------------------------------------+
| Task: 1120-S Tax Preparation - Acme LLC                        [High] [Open]   |
| Client: Acme Holdings | Entity: Acme LLC | Period: 2026 | Code TX-AC-2026      |
| <Edit> <Move stage> <Log time> <Add review point> <Complete task>              |
+--------------------------------------------------------------------------------+
| Current stage: First Review                                                    |
| Waiting on client: [No v]                                                       |
| Original due: Mar 15, 2027 | Extended due: Sep 15, 2027 | Rule: IRS 1120-S     |
+--------------------------------------------------------------------------------+
| | Summary | Checklist | Review Points | Time | Notes | Attachments | History | |
+--------------------------------------------------------------------------------+
| Summary                                                                        |
| +---------------------------------------+ +------------------------------------+ |
| | Assignment                            | | Progress                           | |
| | Preparer: A. Patel                    | | Status: In progress                | |
| | Reviewer: R. Shah                     | | Stage age: 2 days                  | |
| | Partner: A. Runwal                    | | Internal elapsed: 4 days           | |
| | Responsible: N. Mehta                 | | Client wait elapsed: 1 day         | |
| +---------------------------------------+ +------------------------------------+ |
|                                                                                |
| +---------------------------------------+ +------------------------------------+ |
| | Work attributes                       | | Time                               | |
| | Task type: 1120-S                     | | Estimated hours: 14.0              | |
| | Difficulty: Complex                   | | Actual hours: 9.75                 | |
| | Priority: High                        | | Remaining estimate: [4.25]         | |
| +---------------------------------------+ +------------------------------------+ |
+--------------------------------------------------------------------------------+
```

### Checklist tab

```text
+--------------------------------------------------------------------------------+
| Checklist: 1120-S preparation template                         <Add item>      |
+--------------------------------------------------------------------------------+
| +------------------------------------------------------------------------------+
| | Done | Item                         | Owner | Carry fwd | Sign-off | Thread |
| | [x]  | Trial balance imported       | A.P.  | [ ]       | A.P. 7/21| 0      |
| | [ ]  | Officer compensation reviewed| R.S.  | [x]       | -        | 2      |
| | [ ]  | State apportionment checked  | R.S.  | [x]       | -        | 1      |
| +------------------------------------------------------------------------------+
|                                                                                |
| Selected item detail                                                           |
| +------------------------------------------------------------------------------+
| | Officer compensation reviewed                                               |
| | Comments:                                                                    |
| | R.S. Jul 22: Please reconcile W-2 detail.                                    |
| | A.P. Jul 23: Uploaded reconciliation.                                        |
| | [Write a reply...] <Post>                                                    |
| | <Sign off item>                                                              |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### Review points tab

```text
+--------------------------------------------------------------------------------+
| Review points                                               <Add review point> |
+--------------------------------------------------------------------------------+
| {Status: Open v} {Carry-forward: All v}                                        |
| +------------------------------------------------------------------------------+
| | # | Status | Review point               | Raised by | Owner | Carry fwd | > |
| | 1 | Open   | Missing fixed asset detail | R.S.      | A.P.  | [x]       | > |
| | 2 | Resp.  | Confirm state filing       | R.S.      | N.M.  | [ ]       | > |
| +------------------------------------------------------------------------------+
|                                                                                |
| Review point drawer                                                            |
| +------------------------------------------------------------------------------+
| | Missing fixed asset detail                                                   |
| | Severity: Medium | Carry-forward: Yes                                        |
| | Thread:                                                                       |
| | R.S. Jul 22: Please request updated depreciation schedule.                    |
| | A.P. Jul 23: Client provided schedule; attached.                              |
| | [Response...] <Post response> <Resolve>                                       |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### History tab

```text
+--------------------------------------------------------------------------------+
| Immutable stage and activity history                         {Type: All v}     |
| Time              User        Event                                             |
| Jul 24 10:42      R. Shah     Stage moved Preparation -> First Review           |
| Jul 24 10:43      R. Shah     Review point #1 created                           |
| Jul 24 11:05      A. Patel    Waiting on Client changed No -> Yes               |
| Jul 25 09:12      A. Patel    Waiting on Client changed Yes -> No               |
+--------------------------------------------------------------------------------+
```

### Key requirements covered

- Client and entity information are carried through from the engagement.
- Task type, period/tax year, difficulty, estimated hours, priority, staffing, due dates, progress, notes, comments, attachments, time tracking, checklists, and sign-off are visible.
- Waiting on Client is separate from workflow stage.
- Every stage movement is immutable and includes actor, previous stage, new stage, and timestamp.
- Review points and checklist items each have independent threads and carry-forward flags.

## 17. Screen W12 - Workflow stage movement modal

```text
+----------------------------------------------------------+
| Move task stage                                          |
|----------------------------------------------------------|
| Task: 1120-S Tax Preparation - Acme LLC                  |
| Current stage: First Review                              |
|                                                          |
| Move to {Workflow stage * v}                             |
| Options include any configured stage, not just next step. |
|                                                          |
| Waiting on client                                        |
| {Waiting on client: No / Yes v}                          |
| If yes: [Reason / client request details              ]  |
|                                                          |
| [Comment required for backward movement or completion]   |
|                                                          |
| <Cancel> <Move stage>                                    |
+----------------------------------------------------------+
```

### Behavior

- Stage list is configurable by service/workflow template.
- Movement is non-linear: any stage to any other permitted stage.
- Backward movement may require a comment based on admin rules.
- Waiting on Client can be toggled independently from the stage.
- Save creates immutable history.
- Related users receive notifications when configured.

## 18. Screen W13 - Task rollover and recurring setup

```text
+--------------------------------------------------------------------------------+
| Rollover / recurring setup - 1120-S Tax Preparation - Acme LLC                 |
+--------------------------------------------------------------------------------+
| Rollover next period/year                                                       |
| (x) Create next-year task from this task                                        |
| [Next period / tax year *: 2027]                                                |
| [ ] Carry forward staffing                                                      |
| [ ] Carry forward checklist items marked permanent                              |
| [ ] Carry forward review points marked permanent                                |
| [ ] Recalculate statutory due dates                                             |
|                                                                                |
| Preview                                                                         |
| +------------------------------------------------------------------------------+
| | Field                        | Current task        | New task                |
| | Preparer                     | A. Patel            | A. Patel               |
| | Reviewer                     | R. Shah             | R. Shah                |
| | Checklist carry-forward items| 2                   | 2                      |
| | Review points carry-forward  | 1                   | 1                      |
| | Original due date            | Mar 15, 2027        | Mar 15, 2028           |
| +------------------------------------------------------------------------------+
|                                                                                |
| Recurring schedule                                                              |
| ( ) None                                                                        |
| ( ) Monthly on day [15] starting [Aug 2026] ending [Optional]                   |
| ( ) Quarterly on [Month/Day]                                                    |
|                                                                                |
| <Cancel> <Create rollover task> <Save recurring schedule>                      |
+--------------------------------------------------------------------------------+
```

### Key requirements covered

- Year-on-year rollover.
- Automatic recurring task creation.
- Carry-forward logic for staffing, checklists, and review points.
- Due date recalculation from rule engine.

## 19. Screen W14 - Time entry

```text
+--------------------------------------------------------------------------------+
| Time                                                   <Add time entry>         |
+--------------------------------------------------------------------------------+
| | My time | Team time | Approvals |                                               |
+--------------------------------------------------------------------------------+
| My time - week of Jul 20, 2026                                               |
| +------------------------------------------------------------------------------+
| | Date | Task / Client        | Service | Hours | Billable? | Notes | Action   |
| | Mon  | 1040 - Smith         | Tax     | 2.50  | Yes       | ...   | Edit     |
| | Tue  | Books - Bright       | Books   | 3.00  | Yes       | ...   | Edit     |
| | Wed  | Admin training       | Admin   | 1.00  | No        | ...   | Edit     |
| +------------------------------------------------------------------------------+
| Total: 34.75 hours                         <Submit week>                       |
|                                                                                |
| Quick entry                                                                    |
| [Task search *] [Date *] [Start time] [End time] [Hours *]                     |
| [Notes * if non-billable or over estimate]                                     |
| <Start timer> <Save time>                                                      |
+--------------------------------------------------------------------------------+
```

### Team time tab

```text
+--------------------------------------------------------------------------------+
| Team time                                      [Date range] {Staff v} {Client v}|
| +------------------------------------------------------------------------------+
| | Staff | Client / Task | Est hrs | Actual hrs | Variance | Last entry | >     |
| | A.P.  | Acme 1120-S   | 14.0    | 16.5       | ! +2.5   | Jul 24     | >     |
| | R.S.  | Bright Payroll| 6.0     | 4.5        | -1.5     | Jul 23     | >     |
| +------------------------------------------------------------------------------+
| <Export CSV>                                                                   |
+--------------------------------------------------------------------------------+
```

### Key requirements covered

- Individual task time recording.
- Management review of productivity, engagement effort, budgeted versus actual hours, and utilization.
- Filtering and export.

## 20. Screen W15 - Staff management

```text
+--------------------------------------------------------------------------------+
| Staff                                                  <Add staff member>       |
+--------------------------------------------------------------------------------+
| [Search staff] {Status: Active v} {Role v} {Manager v}                         |
| +------------------------------------------------------------------------------+
| | Name | Role | Designation | Manager | Open tasks | Utilization | Status | >  |
| | A. Patel | Staff | Associate | N. Mehta | 24 | 82% | Active | >          |
| | R. Shah  | Reviewer | Manager | A. Runwal | 18 | 70% | Active | >        |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### Staff profile

```text
+--------------------------------------------------------------------------------+
| Staff: A. Patel                                      [Active] <Edit>           |
+--------------------------------------------------------------------------------+
| | Profile | Assignments | Time | Activity |                                      |
+--------------------------------------------------------------------------------+
| Profile                                                                        |
| Role: Staff                                                                    |
| Designation: Associate                                                         |
| Reports to: N. Mehta                                                           |
| Permissions: Preparer, Time entry, Assigned task view                          |
|                                                                                |
| Assignments                                                                    |
| +------------------------------------------------------------------------------+
| | Task | Client | Role on task | Due date | Stage | Action                     |
| | 1040 Smith | Smith | Preparer | Jul 24 | Preparation | Open                  |
| | Books Bright | Bright | Preparer | Aug 15 | Awaiting Info | Open             |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### Central reassignment modal

```text
+----------------------------------------------------------+
| Reassign work                                            |
|----------------------------------------------------------|
| From staff: [A. Patel]                                   |
| To staff:   {Select replacement * v}                     |
|                                                          |
| Apply to roles                                           |
| [x] Preparer assignments                                 |
| [ ] Reviewer assignments                                 |
| [ ] Partner assignments                                  |
| [x] Responsible person assignments                       |
|                                                          |
| Scope                                                    |
| (x) Open tasks and active engagements only               |
| ( ) All future recurring schedules                       |
| ( ) Selected clients / services                          |
|                                                          |
| Preview: 24 tasks, 3 engagements, 2 recurring schedules  |
| [Reason *]                                               |
|                                                          |
| <Cancel> <Reassign and notify affected users>            |
+----------------------------------------------------------+
```

### Deactivation flow

```text
+----------------------------------------------------------+
| Deactivate staff member                                  |
|----------------------------------------------------------|
| Staff: A. Patel                                          |
|                                                          |
| ! This staff member has open assignments.                 |
| Open tasks: 24                                           |
| Active engagements: 3                                    |
| Recurring schedules: 2                                   |
|                                                          |
| <Go to reassignment> <Cancel>                            |
+----------------------------------------------------------+
```

### Key requirements covered

- Staff are first-class entities.
- Profiles capture role, designation, and reporting relationships.
- Central reassignment propagates consistently.
- Deactivation requires clean reallocation of open work.

## 21. Screen W16 - Reports

```text
+--------------------------------------------------------------------------------+
| Reports                                                                         |
+--------------------------------------------------------------------------------+
| Report library                                                                  |
| +------------------------------------------------------------------------------+
| | Report name                    | Description                         | Open  |
| | Tasks due today                | Current-day due work                 | Open  |
| | Overdue engagements/tasks      | Items past due                       | Open  |
| | Work by staff member           | Assignment and workload              | Open  |
| | Pending reviews                | Tasks awaiting reviewer action       | Open  |
| | Work completed                 | Completed task trend                 | Open  |
| | Client-wise workload           | Workload grouped by client           | Open  |
| | Service-wise workload          | Workload grouped by service          | Open  |
| | Productivity report            | Time and throughput by staff         | Open  |
| | Time utilization report        | Utilization by staff/team            | Open  |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### Report detail

```text
+--------------------------------------------------------------------------------+
| Report: Pending reviews                                 <Export CSV> <Schedule> |
+--------------------------------------------------------------------------------+
| Filters                                                                        |
| [Date range] {Client v} {Service v} {Reviewer v} {Priority v} {Stage v}        |
| [ ] Include waiting-on-client tasks                                             |
| <Run report> <Save filter>                                                     |
|                                                                                |
| Results                                                                        |
| +------------------------------------------------------------------------------+
| | Client | Entity | Task | Reviewer | Age in stage | Due date | Priority | >   |
| | Acme   | Acme LLC | 1120-S | R. Shah | 2 days | Jul 25 | High | >       |
| | Bright | Bright PLLC | Books | N. Mehta | 1 day | Aug 15 | Medium | >    |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### Key requirements covered

- Real-time management reports.
- Filtering, exporting, and future customization.
- Reports separate internal workflow time from Waiting on Client where needed.

## 22. Screen W17 - Notifications center

```text
+--------------------------------------------------------------------------------+
| Notifications                                            <Mark all read>        |
+--------------------------------------------------------------------------------+
| Filters: {Unread v} {Type v} {Date range v}                                    |
| +------------------------------------------------------------------------------+
| | Unread | Time | Type | Message | Action                                      |
| | * | 10:42 | Stage movement | Acme 1120-S moved to First Review | Open task   |
| | * | 09:20 | Assignment | You were assigned as reviewer on Bright Payroll | Open|
| |   | Jul 23 | Due date | Smith 1040 due tomorrow | Open task                   |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### Notification preferences

```text
+----------------------------------------------------------+
| Notification preferences                                |
|----------------------------------------------------------|
| In-app                                                   |
| [x] Task assigned or moved to me                         |
| [x] Stage movement on tasks where I am preparer          |
| [x] Stage movement on tasks where I am reviewer          |
| [x] Stage movement on tasks where I am partner           |
| [x] Due date reminders                                   |
|                                                          |
| Email                                                    |
| [x] Task assignment                                      |
| [ ] Every stage movement                                 |
| [x] Daily due date digest                                |
|                                                          |
| <Save preferences>                                       |
+----------------------------------------------------------+
```

### Key requirements covered

- In-app and email notification model.
- Assignment, movement, and due date reminder events.
- Role-specific notification subscriptions.

## 23. Screen W18 - Admin configuration

```text
+--------------------------------------------------------------------------------+
| Admin Configuration                                                             |
+--------------------------------------------------------------------------------+
| | Services | Workflows | Roles | Priorities | Task templates | Checklists |     |
| | Statuses | Due date rules | Code formulas | Settings |                         |
+--------------------------------------------------------------------------------+
```

### Services and sub-services

```text
+--------------------------------------------------------------------------------+
| Services                                                    <Add service>       |
| +------------------------------------------------------------------------------+
| | Code | Service | Sub-services | Workflow | Task templates | Status | Edit    |
| | TAX  | Tax preparation | 1040,1120,1065,1120-S | Tax workflow | 8 | Active | |
| | BK   | Bookkeeping | Monthly, Quarterly | Books workflow | 3 | Active |      |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### Workflow configuration

```text
+--------------------------------------------------------------------------------+
| Workflow: Tax workflow                                  <Add stage> <Save>      |
+--------------------------------------------------------------------------------+
| Service: {Tax preparation v}                                                    |
|                                                                                |
| Stages                                                                         |
| +------------------------------------------------------------------------------+
| | Order | Stage name          | Active | SLA target | Completion stage | Edit  |
| | 1     | Awaiting Information| Yes    | -          | No               | Edit  |
| | 2     | Preparation         | Yes    | 3 days     | No               | Edit  |
| | 3     | First Review        | Yes    | 2 days     | No               | Edit  |
| | 4     | Second Review       | Yes    | 2 days     | No               | Edit  |
| | 5     | Draft Sent          | Yes    | -          | No               | Edit  |
| | 6     | Client Approval     | Yes    | -          | No               | Edit  |
| | 7     | Filing / Completion | Yes    | 1 day      | Yes              | Edit  |
| +------------------------------------------------------------------------------+
|                                                                                |
| Rules                                                                          |
| [x] Allow movement from any stage to any other stage                            |
| [x] Require comment when moving backward                                        |
| [x] Track Waiting on Client outside workflow stage                              |
+--------------------------------------------------------------------------------+
```

### Task template configuration

```text
+--------------------------------------------------------------------------------+
| Task template: 1120-S tax preparation                   <Save template>         |
+--------------------------------------------------------------------------------+
| Service: Tax preparation                                                        |
| Task type: 1120-S                                                               |
| Default workflow: Tax workflow                                                  |
| Default difficulty: {Medium v}                                                  |
| Default estimated hours: [14.0]                                                 |
| Default priority: {Medium v}                                                    |
| Due date rule: {IRS 1120-S original and extension v}                            |
|                                                                                |
| Generated tasks for engagement                                                  |
| +------------------------------------------------------------------------------+
| | Task name | Offset / due rule | Required | Default checklist | Edit          |
| | Intake and source document review | Engagement start + 3 days | Yes | Intake |
| | Prepare return | IRS due rule - 20 days | Yes | 1120-S Prep | Edit          |
| | First review | Prepare complete + 2 days | Yes | Review | Edit             |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### Checklist template configuration

```text
+--------------------------------------------------------------------------------+
| Checklist template: 1120-S Prep                         <Add item> <Save>      |
+--------------------------------------------------------------------------------+
| +------------------------------------------------------------------------------+
| | Order | Item text                         | Default owner | Carry fwd default |
| | 1     | Trial balance imported            | Preparer      | No                |
| | 2     | Officer compensation reviewed     | Reviewer      | Yes               |
| | 3     | State apportionment checked       | Reviewer      | Yes               |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### Due date rules

```text
+--------------------------------------------------------------------------------+
| Due date rules                                            <Add rule>            |
| +------------------------------------------------------------------------------+
| | Rule | Task type | Entity type | Original due | Extension due | Status | Edit |
| | IRS 1120-S | 1120-S | S-Corp | 15th day 3rd month | 15th day 9th month | Active |
| | IRS 1065   | 1065   | Partnership | 15th day 3rd month | 15th day 9th month | Active |
| +------------------------------------------------------------------------------+
|                                                                                |
| Override permissions                                                           |
| [x] Administrators can override calculated due dates                            |
| [x] Managers can request override with reason                                   |
+--------------------------------------------------------------------------------+
```

### Code generation formulas

```text
+--------------------------------------------------------------------------------+
| Code generation formulas                               <Add formula> <Save>    |
+--------------------------------------------------------------------------------+
| Client code formula                                                           |
| Pattern: [Client initials]-[Onboarding year]-[Sequence]                         |
| Example preview: MF-2026-001                                                    |
|                                                                                |
| Service code formula                                                           |
| Pattern: [Service code]-[Client initials]-[Year]                                |
| Example preview: TAX-AC-2026                                                    |
|                                                                                |
| Validation                                                                      |
| [x] Enforce uniqueness by tenant                                                |
| [x] Lock generated code after confirmation                                      |
+--------------------------------------------------------------------------------+
```

### Key requirements covered

- Services and sub-services.
- Workflow stages.
- Roles and permissions.
- Priorities and difficulty ratings.
- Task templates and checklist templates.
- Status values.
- Due date rules.
- Code generation formulas.
- Business configuration without developer involvement.

## 24. Screen W19 - Roles, permissions, and audit

```text
+--------------------------------------------------------------------------------+
| Roles and permissions                                      <Add role>           |
+--------------------------------------------------------------------------------+
| +------------------------------------------------------------------------------+
| | Permission area     | Admin | Partner | Manager | Reviewer | Staff           |
| | View all clients    | [x]   | [x]     | [x]     | [ ]      | [ ]             |
| | View assigned data  | [x]   | [x]     | [x]     | [x]      | [x]             |
| | Reveal sensitive ID | [x]   | [x]     | [ ]     | [ ]      | [ ]             |
| | Override due dates  | [x]   | [ ]     | [ ]     | [ ]      | [ ]             |
| | Configure workflows | [x]   | [ ]     | [ ]     | [ ]      | [ ]             |
| | Export reports      | [x]   | [x]     | [x]     | [ ]      | [ ]             |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### Audit log

```text
+--------------------------------------------------------------------------------+
| Audit Log                                                <Export if permitted>  |
+--------------------------------------------------------------------------------+
| [Date range] {Actor v} {Object type v} {Event type v} {Sensitive access v}     |
| +------------------------------------------------------------------------------+
| | Time | Actor | Object | Event | Before | After | IP / Device |               |
| | Jul24 10:42 | R.Shah | Task AC-1120S | Stage moved | Prep | Review | ...     |
| | Jul24 11:01 | A.Runwal | Client Acme | Sensitive ID revealed | Masked | Shown |
| +------------------------------------------------------------------------------+
+--------------------------------------------------------------------------------+
```

### Key requirements covered

- Role-based access control.
- Staff can access only relevant assigned work.
- Sensitive data access is audited.
- Complete audit trail for important actions.

## 25. Screen W20 - Mobile responsive task view

```text
+--------------------------------------+
| Task: 1120-S - Acme LLC       [High] |
| Stage: First Review                  |
| Waiting on client: No                |
| Due: Sep 15, 2027                    |
|                                      |
| <Move stage> <Log time>              |
|                                      |
| Assignment                           |
| Preparer: A. Patel                   |
| Reviewer: R. Shah                    |
| Partner: A. Runwal                   |
|                                      |
| Tabs                                 |
| [Summary] [Checklist] [Review]       |
| [Time] [History]                     |
|                                      |
| Review points                        |
| 1. Missing fixed asset detail        |
|    Status: Open                      |
|    <Open thread>                     |
+--------------------------------------+
```

### Responsive behavior

- Desktop side navigation collapses to hamburger navigation.
- KPI cards stack vertically.
- Large tables become card lists with essential fields and filter drawers.
- Primary actions remain sticky at the bottom on task-heavy mobile screens.
- Sensitive values remain masked on all breakpoints.

## 26. Critical UI states

### Empty states

| Area | Empty state |
| --- | --- |
| Clients | "No clients found. Add your first client or clear filters." |
| Tasks | "No tasks match this view. Try another saved view or create a manual task." |
| Review points | "No review points yet. Add a review point when reviewer clarification is needed." |
| Time | "No time logged for this period." |
| Reports | "Run a report to view results." |

### Error and warning states

| Scenario | UI response |
| --- | --- |
| Missing due date rule | Show warning in proposal/task preview and require manual due date or admin rule selection. |
| Staff deactivation with open work | Block deactivation until reassignment flow is completed. |
| Unauthorized sensitive field reveal | Deny reveal and log denied access attempt. |
| Backward workflow movement | Require reason if configured. |
| Generated code collision | Show formula conflict and require admin correction before save. |
| Task completion with unsigned required checklist items | Block completion and show outstanding items. |

## 27. Phase 1 coverage matrix

| RFP requirement | Wireframe coverage |
| --- | --- |
| Client Management | W04, W05 |
| System-generated client codes | W04, W18 |
| Entity Management | W05, W06 |
| Engagement Management | W07, W08, W09 |
| Proposal-to-engagement flow | W07, W08 |
| Generated service codes | W07, W18 |
| Task Management | W10, W11 |
| Review points and checklists | W11 |
| Carry-forward flags | W11, W13, W18 |
| Comment thread per review/checklist item | W11 |
| Sign-off with timestamp | W11 |
| Year-on-year rollover | W13 |
| Recurring task creation | W13 |
| Due date automation and overrides | W06, W10, W13, W18 |
| Configurable workflows | W12, W18 |
| Non-linear workflow movement | W12, W18 |
| Separate Waiting on Client status | W02, W10, W11, W12, W16 |
| Immutable stage history | W11, W24 |
| Staff management | W15 |
| Central reassignment | W15 |
| Staff exit/deactivation | W15 |
| Time tracking | W14 |
| Notifications | W17 |
| Admin and staff dashboards | W02, W03 |
| Reporting and exports | W16 |
| Master data and configuration | W18 |
| Secure authentication and RBAC | W01, W19 |
| Audit trail | W05, W11, W19 |
| Sensitive data readiness | W05, W06, W19 |
| Multi-tenancy readiness | Global assumptions, W18, W19 |

## 28. Open product decisions before visual design

1. Confirm which Phase 1 services and task types must ship first, especially among bookkeeping, payroll, 1040, 1120, 1065, 1120-S, advisory, international tax, and compliance.
2. Confirm exact client and entity field definitions, including which sensitive identifiers are deferred versus structurally prepared.
3. Confirm the first set of workflow templates and whether different tax task types need separate stage sequences.
4. Confirm role names and permission boundaries for partner, manager, reviewer, preparer, and administrator.
5. Confirm due date rule details, extension logic, and override approval requirements.
6. Confirm report export formats and whether scheduled reports are in Phase 1 or future configuration.
7. Confirm email notification provider and whether email templates are configurable in Phase 1.

## 29. Suggested next artifact sequence

1. Clickable prototype built from these screens.
2. Data model and entity relationship diagram.
3. API contract for clients, entities, engagements, tasks, workflow events, time entries, and audit events.
4. Role and permission matrix for implementation.
5. Task template and due date rule seed data.
6. Acceptance criteria for each Phase 1 workflow.
