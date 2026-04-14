---
type: procedure
created: 2026-03-22
status: active
slug: notion-expertise-plan
tags: [procedure, nexus]
---

# Notion Expertise Learning Plan
**Generated from Cortex Training Content — 2026-03-05**
**Source: 3 batches — databases/formulas/views, automation/API/templates, project management/teams/calendar**

---

## TOP 20 KEY INSIGHTS

### Database & Structural Insights

**1. Two-Way Relations Are the Foundation of a Relational Workspace**
When creating a relation between databases, always enable the "two-way relation" toggle. This means updating either side of the relation propagates changes to both databases automatically. For the Nexus system: link Procedures DB → Projects DB → Tasks DB as a relational chain, not isolated silos.

**2. Rollups Require Relations First — In That Order**
Rollups cannot exist without a pre-existing relation. The correct build sequence is: (1) create both databases, (2) add a Relation property pointing from DB-A to DB-B, (3) then add a Rollup property that aggregates data from the related DB. Use rollup "calculate" functions: Sum, Average, Count, Percent Checked — not just display.

**3. One Database, Multiple Views = Zero Redundancy**
The single most powerful Notion pattern: create ONE master database with all properties, then create filtered/sorted views for each use case. Example: one Content DB with Table view (editor), Board view (status tracking), Calendar view (deadlines), Gallery view (visual review). Never duplicate databases to serve different views.

**4. AI Autofill Properties Are Context-Aware by Database Type**
Notion AI suggests different properties depending on the database content. A recipes database suggests "preparation time, is vegetarian, recipe file." A finance tracker suggests "account number, account type, account status." The AI autofill (Summary mode) auto-generates descriptions by reading each page's content — activate it on any text-heavy database for instant structured summaries. Costs ~$12/month on the Notion AI plan.

**5. Formula Properties Use Property Names Directly, Not Cell References**
Unlike Google Sheets (where you reference cells like A1/B1), Notion formulas reference property names directly. Syntax: `prop("Income") - prop("Expenses")`. No equal sign needed to start. Use `if(condition, true_value, false_value)` for conditional logic. Double-equals `==` for comparisons. Use AI to generate complex formulas — describe what you want and paste the result.

**6. Number Properties Can Render as Progress Bars or Rings**
Any numeric property can be visually transformed: Edit property → change display to "Bar" or "Ring" → set "Divide by" to your maximum value (e.g., 100 for percentages, 3 for 3-task checklists). This turns raw numbers into visual progress indicators. The bar/ring live-updates as related checkboxes are checked. Critical for task and project tracking dashboards.

**7. Filters Are View-Specific and Non-Destructive**
Applying a filter to a view never deletes or modifies data — it only changes what is visible in that view. The original data remains intact. This means you can create "My Tasks" view (filter: assignee = me), "This Week" view (filter: due date = this week), "Urgent" view (filter: priority = high AND status != Done) all from the same database.

**8. The Home Page "Tasks" Widget Aggregates Across Multiple Databases**
The Notion home page has a dedicated "My Tasks" widget that can pull tasks from multiple source databases simultaneously. To configure: add a source database, set filters (assignee contains me OR assignee is empty), and all matching tasks appear in a unified list. You can change status, navigate to the source, or create new tasks directly from the home page without opening individual databases.

**9. Synced Blocks Enable Single-Source-of-Truth Content**
A Synced Block is a block that lives in one location but appears identically across multiple pages. Edit it once — all instances update. Use case: team announcements board, FAQ section, standard operating instructions that appear in multiple project pages. This is the Notion equivalent of a transclusion/embed.

**10. Locking Databases Prevents Accidental Structural Changes**
Any database or page can be locked via the three-dot menu → Lock page. Locked pages cannot be edited or restructured. Lock critical reference databases (team goals, procedures, metrics) while keeping working databases editable. Lock does not prevent reading or filtering — only editing.

### Automation & Integration Insights

**11. Notion-Native Database Automations Trigger on Status Changes**
Inside any database, click the edit/settings icon → Automations → New Automation. Trigger: "when property changes" (e.g., Status changes to "Done"). Action: send a Slack message to a specific channel, notify a person, edit another property. This requires the Slack integration to be enabled at Settings → My Connections AND slack notifications to be configured at Settings → My Notifications → Slack Notifications (workspace selector).

**12. Zapier Unlocks 4000+ App Connections to Notion**
Notion's native integrations cover ~200 tools. Via Zapier, this expands to 4000+ apps. Key Zapier → Notion use cases:
- Gmail → Notion: auto-create database items from incoming emails matching specific criteria
- Google Forms → Notion → Gmail → Slack: form submission creates DB item, triggers confirmation email, notifies team channel
- Notion → ChatGPT → Notion: add an idea to column A, Zapier sends it to ChatGPT with a prompt, result writes back into the database
Zapier trigger options for Notion: new database item, database item edited, page updated.

**13. Notion Forms Replace Typeform/Google Forms Natively**
Notion Forms (available on free plan) create a public-facing form that writes submissions directly into a connected Notion database. Create: New page → Form button. The form automatically generates fields from the database properties. Share via link. All submissions appear as database rows — immediately filterable, sortable, and viewable in any database view. Replaces: job applications, feedback collection, onboarding forms.

**14. Notion Calendar Is a Separate App That Syncs Google Calendar**
Notion Calendar is not just a database view — it is a standalone desktop/mobile app. It integrates your Google Calendar events AND your Notion database dates into one unified calendar view. Setup: Settings → Notion Calendar → Connect workspace + Google account. Once connected, calendar events appear on the Notion home page widget. Toggle individual calendars on/off for planning focus.

**15. Buttons Automate Workspace Actions Without Zapier**
The Notion Buttons feature (in the course's dedicated section) allows creating clickable buttons that trigger automated actions: create a new page in a specific database, add a block, update a property, navigate to another page. Buttons are placed on any page. Use case: "Start Daily Review" button that creates a new journal entry with today's date pre-filled, or "New Task" button that creates a task in the master Tasks DB with default status = "To Do".

### Workspace Design Insights

**16. Team Spaces Are the Correct Organizational Layer**
Workspace → Team Spaces → Pages is the correct hierarchy. Create team spaces per department or function (Marketing, Dev, Finance). Assign roles: Admin (full control, can edit settings and invite), Member (can edit content), Guest (page-only access). Keep admin roles to a minimum — too many admins leads to structural drift. Guests cannot access team spaces at all without explicit invitation to specific pages.

**17. Standardized Naming Conventions Prevent Search Chaos**
At scale, naming inconsistency destroys findability. Convention patterns that work:
- `[DEPT]-[ProjectName]` (e.g., `MKTG-GamblingSEO-Peru`)
- `[Status emoji] [Name]` (e.g., `🟢 Live Campaign`, `🔴 Blocked Task`)
- Use emoji prefixes per category: palette for design, chart for analytics, wrench for dev
Enforce naming at workspace setup — changing conventions retroactively is high-friction.

**18. The /remind Command Creates Date-Linked Notifications Anywhere**
Two ways to create reminders: (1) click any date property → Remind → choose timing (day of, 1 day before, 1 week before, at time, 10 min before, etc.); (2) type `/remind` anywhere on any page to create a standalone reminder block not tied to any property. Method 2 is powerful: reminder a page check-in on any future date (e.g., "revisit gambling SEO strategy June 1"). Notifications arrive via Notion inbox, email, and push notification.

**19. Gallery View Requires a Files & Media Property to Be Useful**
Gallery view does not render meaningfully unless at least one property is of type "Files & Media" — this becomes the gallery card image. Without it, gallery shows blank cards. When you have image/thumbnail properties, gallery becomes a visual content board — ideal for content planning, product catalogs, mood boards.

**20. Horizontal Database Scrolling Requires Shift+Scroll**
When a database has many properties and extends horizontally beyond the screen, the non-obvious shortcut is: hold Shift + scroll with mouse wheel to scroll horizontally. Without knowing this, users either drag the scrollbar or miss properties entirely. Additional UX tip: hide vertical lines in databases (three dots → Layout → toggle "Show vertical lines" off) for a cleaner, more readable table.

---

## NOTION EXPERTISE ROADMAP

### Level 1: Core Fundamentals
**Target: Can build usable, organized workspaces**

**1.1 Pages & Blocks**
- Understanding the Pages + Blocks model ("Lego" metaphor)
- Creating pages, sub-pages, nested hierarchies
- Block types: text, headings, toggle, callout, divider, code, quote, bulleted/numbered lists
- Slash command (`/`) to insert any block type
- Using `@` to mention people, dates, or pages
- Cover images, icons, full-width toggle
- Mobile vs desktop behavior differences

**1.2 Databases — First Contact**
- What a database is (rows = pages, columns = properties)
- Creating a new empty database
- Property types: Title, Text, Number, Select, Multi-Select, Date, Checkbox, URL, Email, Phone, Files & Media, Person, Relation
- Adding/editing/deleting properties
- Filling rows with data
- Opening individual database rows as full pages
- The importance of the Title property (it is the page name)

**1.3 The Six Database Views**
- Table view: primary editing surface, all properties visible
- Board view: Kanban by any Select/Status property — shows columns as pipeline stages
- List view: minimal, clean, for reading
- Calendar view: requires a Date property, shows items on calendar grid
- Timeline view: requires Start + End date properties, Gantt-chart style
- Gallery view: requires Files & Media property, visual card grid
- Creating multiple views on one database; switching between them
- Understanding that views never change underlying data

**1.4 Workspace Navigation**
- Left sidebar: favorites, team spaces, private pages, shared pages
- Search (`Cmd+K`): full-text search across entire workspace
- Settings: profile, notifications, connections
- Favorites/starred pages for quick access
- Lock page/database to prevent editing

**Milestone:** Build a working personal task database with at minimum 3 views (Table, Board, Calendar) and all tasks organized.

---

### Level 2: Advanced Features
**Target: Can build interconnected, self-calculating systems**

**2.1 Relations**
- Why relations exist: link separate databases without copy-paste
- Creating a Relation property: choose target database, select two-way vs one-way
- Two-way: changes in either DB propagate to both
- Relation limit: set to "no limit" unless you specifically need 1:1
- Use case: Projects DB ↔ Tasks DB ↔ People/Assignees DB

**2.2 Rollups**
- Prerequisite: a Relation must already exist
- Creating a Rollup property: select the relation, select the property to pull from, select the calculation (Show original, Count all, Count values, Count unique, Count checked, Sum, Average, Median, Min, Max, Range, Percent checked, Percent unchecked)
- Live-updating: rollup recalculates when related records change
- Use case: total expense per category, average task completion rate per project

**2.3 Formulas**
- Formula is a property type, not a cell operation
- Syntax: `prop("PropertyName")` to reference other properties
- Arithmetic: `+`, `-`, `*`, `/`
- Comparison operators: `==`, `!=`, `>`, `<`
- Logical: `if(condition, trueValue, falseValue)`, `and()`, `or()`, `not()`
- Text: `format()`, `contains()`, `replace()`, `length()`
- Date: `now()`, `dateBetween()`, `dateAdd()`
- For advanced formulas: use ChatGPT with the prompt "Write a Notion formula that [description]. The properties available are: [list]"
- Number format: change display to %, currency, number with commas

**2.4 Filtering, Sorting, Grouping**
- Filters are view-specific — original data unchanged
- Filter rules: property + condition + value (e.g., Status is not Done)
- Advanced filters: AND / OR compound rules
- Sorting: primary sort + secondary sort
- Grouping: group by any Select/Status property — creates column groups in any view
- "No group" view vs grouped views — toggle as needed
- Filter by "Me" for personal task views

**2.5 AI Properties**
- AI-suggested properties appear when adding new property to a database
- AI Autofill modes: Summary, Action items, Key quotes, Translate, Custom
- Enable Auto-update to re-run AI fill whenever the page content changes
- "Autofill all pages" button to batch-process entire database column
- Context-aware: AI suggestions differ by database type (recipes vs finance vs projects)
- Requires Notion AI plan (~$12/month)

**2.6 Visual Property Display**
- Number properties: switch display to Bar or Ring
- Configure "Divide by" value to set the maximum scale
- Use for: project progress (tasks checked / total tasks), ratings (score / 10), completion percentages
- Progress bars live-update when related checkboxes are toggled

**Milestone:** Build a fully relational Projects + Tasks + People database system with rollup-calculated progress bars and at least two formula properties.

---

### Level 3: Power User
**Target: Can automate workflows, integrate external tools, and build systems that run themselves**

**3.1 Native Notion Automations**
- Location: database → settings/edit icon → Automations
- Trigger types: property changes (e.g., Status changes to "Done"), new item added, item deleted, date arrives
- Action types: edit a property, send a Slack notification, notify a person, add a page
- Combine with Slack integration: Settings → My Connections → add Slack, then Settings → My Notifications → Slack Notifications → select workspace
- Use case: when Priority = Critical AND Status changes to In Progress → send Slack message to #alerts

**3.2 Zapier Integration**
- Create free Zapier account (requires work email)
- Go to zapier.com → Explore Apps → search Notion → Connect Notion to 7000+ apps
- Authorize Notion: "Select pages" → select all → Allow access
- Zapier trigger events for Notion: New database item, Database item edited, Page updated
- Key Zapier → Notion zaps:
  - Gmail → Notion: create DB item from emails matching subject/sender criteria
  - Google Forms → Notion → Gmail → Slack: full feedback pipeline in one zap chain
  - Notion → ChatGPT → Notion: add content idea → AI develops it → result writes back
- Use Zapier AI ("tell Zapier what you want") to generate zap outlines automatically
- Notion currently has fewer native triggers than Google Sheets — supplement with Zapier

**3.3 Notion Forms**
- Create: New page → Form button (available on free plan)
- Form auto-generates questions from the connected database's properties
- Share via public link — no Notion account required to submit
- All submissions appear instantly as database rows
- Viewable in any database view (calendar, board, table) immediately
- Use cases: job applications, client onboarding, feedback collection, procedure requests

**3.4 Notion Calendar App**
- Separate download: desktop (Mac/Windows), iOS, Android, browser
- Sign in with Google → connects your Google Calendar accounts
- Connect to Notion workspace in app Settings → Notion → select workspace → Connect
- Toggle individual Google/Notion calendars on/off for planning context
- Once connected: calendar events appear on Notion home page widget
- Best use: unified view of Google meetings + Notion task deadlines in one calendar

**3.5 Google Drive Integration**
- Go to notion.com/integrations → search Google Drive → Add to Notion
- Connect Google account (can connect multiple accounts)
- Embed Google Drive files directly in Notion pages — no more "where did the file go?"
- Share file links inside Notion without switching apps
- Good for: team resource hub, project file references, shared document library

**3.6 Slack Integration (Detail)**
- notion.com/integrations → Slack → Add to Notion → Allow
- In Notion settings → My Notifications → Slack Notifications → select workspace
- Now: any mention `@[your name]` in Notion → instant Slack DM
- Add database automation: When property changes → Send Slack message to #channel
- Insert dynamic variables in Slack message: `{{Title}}`, `{{Status}}`, `{{Assignee}}`
- This creates fully automated status-update Slack notifications without Zapier

**3.7 Import & Templates**
- Import from: Evernote, Trello, Asana, Google Docs, Confluence, Markdown, CSV, HTML
- Access: New page → three dots → Import, or Settings → Import
- Template Gallery: left sidebar → Templates → browse by category
- Duplicate any template: "Get template" or "Duplicate" → appears in your workspace
- Create custom templates inside any database: inside DB → New → arrow next to button → New template

**3.8 Notion Buttons**
- Insert a Button block anywhere on a page via `/button`
- Configure actions: insert content above/below, create page in database, edit properties, open page, send notification
- Use case: "Start Daily Review" button → creates journal entry with today's date + template content pre-filled
- Use case: "New Nexus Procedure" button → creates page in Procedures DB with status = Draft, type = FORGE
- Buttons run multiple actions in sequence — chain them for workflows

**Milestone:** Have at least 3 active automations running (1 native, 1 Zapier, 1 button-triggered), Slack notifications wired to at least one database, and a working Notion Form collecting data into a database.

---

### Level 4: Expert / Architect
**Target: Can design systems for teams, build second-brain architectures, and integrate AI**

**4.1 Workspace Architecture Design**
- Hierarchy: Workspace → Team Spaces → Pages → Databases
- Team Spaces per domain: create separate spaces for Marketing, Dev, Finance, Research
- Assign roles deliberately: Admin (minimal), Member (default), Guest (per-page)
- Use workspace settings → Members → roles to manage access control
- Private pages vs shared pages: understand the difference for personal vs team content
- Team wiki pages: centralize branding assets, SOPs, contact directories, shared calendars

**4.2 Master Database Pattern**
- Build ONE master database per entity type (Tasks, Projects, Procedures, Contacts)
- Create specialized views that filter/group for specific team roles or workflows
- Example: one Tasks DB → "My Tasks" view (filtered: me), "Team Board" view (grouped by assignee), "This Week" view (filtered: due date this week), "Backlog" view (filtered: status = not started, sort: priority desc)
- Lock master database schemas from non-admins

**4.3 Notion Home Page as Operations Dashboard**
- Home page is desktop-only (not mobile/iPad)
- Widget types: Recently Visited, Upcoming Events, My Tasks, Custom Database Views, Journal
- My Tasks widget: configure sources (multiple databases), set filters (assignee = me OR empty), choose layout (list, board, table, timeline)
- Upcoming Events widget: connects to Notion Calendar → shows Google Calendar + Notion date properties
- Add any database view directly to home page for at-a-glance visibility
- Custom greeting, profile completion — use home page as daily command center

**4.4 Synced Blocks for Distributed Content**
- Insert synced block: `/sync` or copy block → paste as synced copy
- Edit once, updates everywhere — ideal for: team announcements, recurring instructions, shared FAQs
- Use for: procedure status dashboards embedded in multiple project pages, Genie system status in multiple contexts
- Avoid for: content that should differ per context

**4.5 Workspace Maintenance System**
- Schedule quarterly workspace audits: dead pages, orphaned databases, missing assignees
- Create a "Workspace Maintenance Checklist" template — duplicate each audit cycle
- Checklist items: all databases have an owner, no duplicate databases, all team members have correct roles, archived pages moved to trash, template gallery is current
- Assign dedicated "workspace admin" role to one team member

**4.6 Notion AI as a Co-Architect**
- AI-powered properties: auto-generate summaries, action items, key quotes for any text-heavy database
- Use AI to suggest database schema when designing new databases
- Use Zapier → ChatGPT → Notion pipeline for: content ideation, procedure generation, meeting note processing
- AI search in Notion: Cmd+K → ask questions in natural language, AI retrieves relevant pages
- Custom AI autofill prompts: write specific instructions for what the AI should extract from each page

**4.7 Role-Based View Architecture for Teams**
- Design views based on what each role NEEDS to see, not what exists
- Video editor view: filtered to tasks with "needs intro = yes", sorted by due date
- Manager view: grouped by assignee, showing all statuses
- Client view: filtered to status = Done or In Review, hiding internal properties
- Lock views to prevent accidental filter/sort changes (use locked views or documented naming conventions)

**4.8 Embedding External Tools**
- Embed Figma, Loom, Google Drive files, YouTube, GitHub, Typeform directly in pages via `/embed`
- Eliminates context switching — design files, videos, forms all accessible in-context
- Use for: project briefs that include design mockups, procedure pages with video walkthroughs, task pages with embedded Loom demos

**Milestone:** A fully operational workspace with: team spaces, role-based access, master databases with relational chain, AI-powered properties, automated Slack notifications, a configured home page dashboard, and at least one external tool embedded per major project type.

---

## TOP 10 NOTION PROCEDURES TO CREATE

**PROC-001: NOTION-DATABASE-DESIGN-PATTERN**
*When and how to design a new Notion database from scratch*
- Trigger: need to track a new entity type (project, task, person, procedure, resource)
- Steps: define entity attributes → map to property types → set up Title property → identify relations to existing DBs → create initial views (Table for editing, Board for status, Calendar for dates) → lock schema
- Includes: property type selection matrix, relation vs tag decision tree, rollup calculation options

**PROC-002: NOTION-RELATION-ROLLUP-SETUP**
*Creating a relational system between two or more databases*
- Trigger: two databases that share data or need aggregated calculations
- Steps: create relation property (specify direction, enable two-way) → verify backlink appears in target DB → create rollup property → select calculation function → test with sample data
- Includes: common relation patterns (Projects→Tasks, Categories→Items, People→Assignments)

**PROC-003: NOTION-FORMULA-CREATION**
*Building formula properties for calculated columns*
- Trigger: need a column whose value is derived from other columns
- Steps: identify input properties → determine operation type (arithmetic/logical/text/date) → write formula using `prop("Name")` syntax → test → format output (%, currency, number)
- Includes: 15 most-used formula templates, ChatGPT prompt for formula generation: "Write a Notion formula that [goal]. Available properties: [list with types]"

**PROC-004: NOTION-ZAPIER-PIPELINE-SETUP**
*Connecting Notion to external apps via Zapier for automation*
- Trigger: need automation between Notion and a non-natively-integrated tool
- Steps: create Zapier account → Explore Apps → connect Notion (authorize all pages) → select trigger event → map fields → add action → test → publish
- Includes: top 5 Nexus-relevant Zapier templates, ChatGPT→Notion pipeline setup, Gmail→Notion data capture zap

**PROC-005: NOTION-SLACK-AUTOMATION**
*Setting up Notion-Slack notification automation for database changes*
- Trigger: need team to be notified in Slack when Notion database items change status
- Steps: add Slack integration → configure Slack notifications in settings → create database automation (trigger: property change) → configure Slack action (channel, message template with dynamic variables) → test
- Includes: automation trigger types, dynamic variable list, recommended automation patterns for teams

**PROC-006: NOTION-HOME-PAGE-DASHBOARD**
*Configuring the Notion home page as an operational command center*
- Trigger: setting up a new workspace or restructuring for daily use
- Steps: open Home → configure My Tasks (add database sources, set filters, choose layout) → configure Upcoming Events (connect Notion Calendar) → add custom database views → pin high-priority databases as starred
- Includes: recommended widget configuration for solo operators, recommended for team leads

**PROC-007: NOTION-AI-PROPERTY-AUTOFILL**
*Using Notion AI to auto-generate structured data in database columns*
- Trigger: have a text-heavy database where manual summarization or extraction is too slow
- Steps: add new property → select AI autofill mode (Summary/Action items/Key quotes/Translate/Custom) → enable Auto-update → run "Autofill all pages" → review and correct edge cases
- Includes: use cases by database type, custom prompt format, cost/plan notes

**PROC-008: NOTION-WORKSPACE-ARCHITECTURE**
*Designing and initializing a multi-team Notion workspace from scratch*
- Trigger: new project, new team, or Nexus workspace restructure
- Steps: define domains → create Team Spaces per domain → configure member roles → establish naming conventions (format standards) → create master databases per entity type → build shared wiki pages → lock critical databases
- Includes: role matrix (Admin/Member/Guest), naming convention templates, mandatory databases list

**PROC-009: NOTION-BUTTON-WORKFLOW-AUTOMATION**
*Creating Button blocks that automate repetitive page/database actions*
- Trigger: recurring action that requires multiple manual steps each time
- Steps: insert Button block (`/button`) → configure trigger text → add action sequence (create page / edit property / insert content) → test → document the button's purpose
- Includes: 5 high-value button templates for Nexus, multi-action chain setup

**PROC-010: NOTION-FORMS-DATA-COLLECTION**
*Setting up Notion Forms to replace external form tools*
- Trigger: need to collect structured data from external parties (clients, team, public)
- Steps: create new page → select Form → configure questions (tied to DB properties) → customize submit message → copy share link → test submission → view results in connected database
- Includes: form field type mapping, embedding forms in external pages, connecting form DB to reporting views

---

## IMMEDIATE ACTION ITEMS

**ACTION 1: Convert Procedures Database to Relational System**
The current Procedures DB (297 procedures) likely has no relation to Projects or Tasks. Implement:
- Add a Relation property linking Procedures → Projects DB (which procedures are used in which projects)
- Add a Rollup property in Projects DB: count of linked procedures
- Create a "Procedures Used" view inside each project page
*Why now:* With 297 procedures, manual cross-referencing is not scalable.

**ACTION 2: Build the Nexus Home Page Dashboard**
Configure the Notion home page as the daily operating dashboard:
- My Tasks widget: add sources = Tasks DB + Procedure Queue + any active project task DB, filter = assignee = me OR empty
- Upcoming Events: connect Notion Calendar + Google Calendar
- Add "Procedures Ready for AUDIT-PRO" as a database view widget (filter: status = Draft, sort: created date)
- Add "Active Projects" board view widget
*Why now:* Currently navigating manually — the home page eliminates sidebar hunting.

**ACTION 3: Set Up Notion → Slack Automation for Procedure Status Changes**
Wire the Procedures DB to Slack:
- Trigger: when Status property changes to "AUDIT-PRO PASS"
- Action: send message to a designated Slack channel with procedure name + link
- Second automation: when Status = "PENDING" → notify in Slack to queue for Codex
*Why now:* Manual status tracking across 297+ procedures is error-prone.

**ACTION 4: Create a "New Procedure" Button on the Main Procedures Page**
Place a Button block at the top of the Procedures DB page:
- Action 1: Create new page in Procedures DB
- Action 2: Set Status = "Draft"
- Action 3: Set Created Date = today
- Action 4: Set Type = "FORGE" (default)
*Why now:* Currently creating each procedure manually — button removes setup friction and enforces defaults.

**ACTION 5: Enable AI Autofill (Summary) on the Procedures Database**
Add an AI property to the Procedures DB:
- Property name: "AI Summary"
- Mode: Summary
- Enable Auto-update: ON
- Run "Autofill all pages" once (will generate summaries for all 297 procedures)
This creates instant searchable, scannable summaries without manual effort.
*Why now:* With 297 procedures, the summary column makes the database scannable in table view without opening individual pages.

---

## BEST PRACTICES SUMMARY

### Database Design
- Build ONE master database per entity type — never duplicate for different views
- Always enable two-way relations between linked databases
- Use Select properties (not Multi-Select) when one item can only belong to one category; use Multi-Select when it can belong to many
- Keep the Title property clean and meaningful — it is the page name everywhere
- Standardize naming conventions before inviting others to a workspace
- Lock database schemas (via "Lock page") once they are stable to prevent accidental property changes

### Views & Filtering
- Create views for each distinct workflow/audience, not for each person
- Name views descriptively: "Editor Board", "My Tasks This Week", "Completed Q1" — not "View 1"
- Filters are view-level, never touch the underlying data
- Group by Status for pipeline tracking; group by Assignee for workload visibility
- Use Calendar view only with databases that have a Date property populated

### Formulas & Calculations
- Reference properties by exact name in quotes: `prop("Amount")`
- Use percent format for ratios, currency format for money — set in property formatting
- Use AI to generate complex formulas — describe the logic in plain language
- Always test formulas with edge cases: null values, division by zero, empty text

### Automation
- Native automations first (simpler, more reliable); Zapier for anything not natively supported
- Keep zaps simple (2-3 steps max when starting); chain multiple simple zaps rather than one complex one
- Always test automations with real data before publishing
- Use dynamic variables in Slack messages to include relevant context (page title, status, assignee)
- Buttons are the fastest form of automation — use them for any 3+ step repetitive action

### Team Collaboration
- Assign minimum necessary roles: most people should be Member, not Admin
- Lock important reference databases and goal/metric pages — guests and members cannot change them
- Use Synced Blocks for announcements and shared instructions — one edit, propagates everywhere
- Use @mentions in Notion instead of messaging in external chat — keeps context attached to the source
- Create a "Team Wiki" page with pinned resources, available to all members
- Schedule quarterly workspace maintenance: audit dead pages, unassigned tasks, redundant databases

### Workspace Organization
- Use emoji prefixes to visually group pages in the sidebar (no text scanning required)
- Archive or trash unused pages rather than leaving them in the sidebar
- Pin (star) only the 5-7 most actively used pages — too many favorites defeats the purpose
- Use the Search (`Cmd+K`) as primary navigation once the workspace grows beyond ~30 pages
- Create a "Start Here" page for new workspace members with navigation guide, naming conventions, and role descriptions

### AI Integration
- AI Autofill properties are most valuable on text-heavy databases (notes, procedures, meeting records)
- Use "Summary" mode for quick scannability; "Action items" mode for meeting notes
- Enable Auto-update on AI properties so they stay current as page content changes
- The Zapier → ChatGPT → Notion pipeline is the most powerful AI integration available without the Notion AI plan cost

---

*Plan generated from 85+ unique transcript sections across 3 content batches. Total source content: ~540,000 characters across databases/formulas/views, automation/integrations, and project management/teams/calendar domains.*
