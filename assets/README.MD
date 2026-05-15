I'll update the todo list to reflect embedding the full Saral Test Application document and then replace README.md with the provided document content, embedding images from assets.

Completed: *Analyze file contents and summarize* (2/5)

Apply Patch

Generating patch (4 lines) in [](file:///c%3A/Users/DELL/Downloads/Dashboard%2015052026/README.md)


```

```
I'll open README.md to confirm the current contents.

Read [](file:///c%3A/Users/DELL/Downloads/Dashboard%2015052026/README.md)

Now I'll replace README.md with the full Saral Test Application guide and embed images from assets.

Apply Patch

I can't write files right now (apply_patch is disabled). Paste the content below into README.md (I'll update the todo after you confirm or re-enable file edits).

---- Begin README content ----
# Saral Test Application — User Features, Functionality & Screenshot Guide

Document Type: User-facing feature and functionality guide

Application Scope: Log Book, Shift Dashboard, Operator Management, Model Master, Login/Dashboard, Role/Page/Category Management, User Management

Source Analyzed: Node-RED flow JSON (`flows (32).json`)

Flow Size: 233 nodes, 7 tabs, 44 HTTP endpoints, 63 function nodes

Database Target Found: MySQL `master` on 127.0.0.1:3306

API Port Found: global `api_port` is set to `1881`

How to use this document
- Replace each screenshot placeholder with the actual UI screenshot after opening the matching route in the browser. Keep the feature tables as user-facing explanations for training, handover, or client review.

1. Application Overview
The uploaded flow represents a Node-RED based web application that serves multiple HTML screens and REST APIs over HTTP. It uses MySQL as the backend database and provides operational dashboards, master data management, role/menu configuration, and user administration.

1.1 Main Modules Identified
| Module | Primary UI Route | Main Users | Core Purpose |
|---|---|---|---|
| Login & Dashboard | /login, /dashboard_user, /admin_login, /dashboard_admin | All users / admins | Authenticate users and load role-based dashboard menus. |
| Log Book Dashboard | /log_books | Production / Quality team | View detailed test logs with search, filters, pagination, PASS/FAIL badges, and Excel export. |
| Shift Analytics Dashboard | /analise | Production supervisors | Show total tested, pass/fail count, running model, model mix, hourly production, and rejection charts. |
| Operator Management | /operator | Admin / supervisor | Create, edit, search, filter, and delete operator records. |
| Model Master | /modelmasterdata | Engineering / quality admin | Maintain model-wise test limits, speed parameters, RPM limits, and optional test blocks. |
| User Management | /userData | Admin | Create, edit, delete, search, and filter application users and assign roles. |
| Page/Role/Category Manager | /page-manager, /role-manager | Admin | Manage dashboard pages, categories, roles, and role-wise page access. |

1.2 Overall User Flow
1. User opens the login page and enters username and password.  
2. System validates the user and reads the role-wise menu/page access.  
3. Dashboard loads the allowed pages in a left/top navigation layout.  
4. Users open operational screens such as Log Book, Shift Dashboard, Operator Management, Model Master, or User Management based on role access.  
5. Admin users manage pages, roles, categories, and user access to control what each role can see.

2. Screenshot Attachment Guide
Use this section while replacing the screenshot placeholders in later pages. The document is prepared with dedicated screenshot boxes for every important UI screen.

2.1 Recommended Screenshot Rules
1. Open the UI route in the browser using the same host as Node-RED and the configured API port, for example: `http://localhost:1881/login`.  
2. Use Windows + Shift + S or the Snipping Tool to capture the visible browser area.  
3. For dashboards, capture the header, filters, charts/cards, and at least the first few rows of data.  
4. For create/edit forms, capture the modal or form after opening it, not only the empty list page.  
5. Paste the screenshot inside the matching placeholder box. Right-click the image in Word and use Wrap Text -> In Line with Text for clean layout.  
6. Keep screenshots readable. Do not compress too much, and avoid cutting off important buttons, table headers, filters, or chart legends.

2.2 Screenshot Checklist
| ID | Screen | Route / Source | Capture focus |
|---|---|---|---|
| SS-01 | User Login | /login | Login card with username/password fields. |
| SS-02 | User Dashboard | /dashboard_user | Dashboard menu after successful login. |
| SS-03 | Super Admin Login | /admin_login | Admin login screen. |
| SS-04 | Super Admin Dashboard | /dashboard_admin | Admin dashboard landing page. |
| SS-05 | Log Book Dashboard | /log_books | Search/filter row, sticky table header, PASS/FAIL statuses. |
| SS-06 | Log Book Export | Downloaded .xls file | Excel export opened with styled header and status colors. |
| SS-07 | Shift Analytics Dashboard | /analise | Summary cards and charts. |
| SS-08 | Operator Management List | /operator | Operator table/cards with filters. |
| SS-09 | Add/Edit Operator Modal | /operator | Operator modal with fields. |
| SS-10 | Model Master List | /modelmasterdata | Existing model table and search. |
| SS-11 | Model Master Form | /modelmasterdata | Add/edit model form with speed/test parameters. |
| SS-12 | User Management | /userData | User table, role filter, and action buttons. |
| SS-13 | Page Manager | /page-manager | Page list and add/edit modal. |
| SS-14 | Role Manager | /role-manager | Role configuration with page/category mapping. |

3. Login, Session & Dashboard
UI Route(s): `/login`, `/dashboard_user`, `/admin_login`, `/dashboard_admin`

Purpose: Provides entry point to the application and displays dashboard navigation according to the user role.

Key Features
| Feature | What it does | User value |
|---|---|---|
| User login | Username and password based login form posts to the login API. | Prevents direct access and personalizes the session. |
| Role-wise menu | Dashboard calls the role menu API and shows only mapped pages. | Users see only the modules assigned to their role. |
| Session storage | User, menu, token, login time, and last activity are stored in browser session storage. | Maintains session state during dashboard usage. |
| Idle timeout logic | The login page includes a five-minute session and idle timeout concept. | Improves session control for shared systems. |
| Dashboard iframe/content area | Selected module pages load inside a dashboard frame/container. | Keeps navigation consistent while switching modules. |

How to Use
1. Open `/login` for normal user login or `/admin_login` for super admin login.  
2. Enter credentials and submit.  
3. After success, dashboard opens with allowed menu items.  
4. Click any menu item to open that module inside the dashboard frame.

Screenshot SS-01 - User Login  
![SS-01 User Login](assets/15.05.2026_13.42.58_REC.png)

Screenshot SS-02 - User Dashboard Menu  
![SS-02 User Dashboard](assets/15.05.2026_15.03.38_REC.png)

Screenshot SS-03/SS-04 - Admin Login and Admin Dashboard  
![SS-03 Admin Login / SS-04 Admin Dashboard](assets/15.05.2026_16.15.23_REC.png)

4. Log Book Dashboard
UI Route(s): `/log_books`

Purpose: Shows detailed production/test log records from the `log_book` table with filtering, pagination, sticky columns, and Excel export.

Key Features
| Feature | What it does | User value |
|---|---|---|
| Search | Searches operator, product code, model, serial number, statuses, and final result. | Quickly find a specific unit, model, operator, or failed condition. |
| Date filtering | From and To date inputs filter records by timestamp. | Helps review a specific shift, day, or period. |
| Pagination and row count | Rows can be shown as 20, 50, 100, 200, or all. | Improves performance and readability for large log data. |
| Sticky table headers and columns | Timestamp, operator, product code, model, and serial number remain visible while scrolling. | Users do not lose context in wide tables. |
| PASS/FAIL badges | PASS and FAIL values are displayed with green/red visual badges. | Quality status becomes easy to scan. |
| Fail row highlight | Rows with final FAIL are highlighted. | Failed units are immediately visible. |
| Excel export | Downloads filtered records in a styled .xls file. | Supports offline reporting, sharing, and audit records. |
| Auto refresh | The screen refreshes the current page every 60 seconds. | Keeps live production status updated. |

How to Use
1. Open `/log_books`.  
2. Use Search to filter by serial number, model, status, or operator.  
3. Select From/To dates and click Apply Filter.  
4. Change Rows count when more records are needed.  
5. Click Export Excel to download the current filtered report.

Screenshot SS-05 - Log Book Dashboard  
![SS-05 Log Book Dashboard](assets/15.05.2026_16.16.29_REC.png)

Screenshot SS-06 - Exported Excel Output  
![SS-06 Exported Excel](assets/15.05.2026_16.21.22_REC.png)

General Info (example rows from exported data)  
| Timestamp | Operator | Product Code | Model | Product SN | LVS | ECR | HV / IR / LC | Stage 1 | Stage 2 | Stage 3 | Swing | Final | RPM |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 15/05/2026, 15:00:12 | Jeevan Singh | 1191 | Skyla I-75 AC | F-Hindware119115052600001-123457 | 123.697 PASS | | | | | | | FAIL | |
| 15/05/2026, 14:59:35 | Jeevan Singh | 1191 | Skyla I-75 AC | F-Hindware119115052600001-123457 | 124.124 PASS | | | | | | | FAIL | |

5. Shift Analytics / Addon Dashboard
UI Route(s): `/analise`

Purpose: Provides a shift-level production analytics view with summary metrics and charts based on `log_book` data.

Key Features
| Feature | What it does | User value |
|---|---|---|
| Total tested count | Displays total tested units for the current shift/day logic. | Gives immediate production volume visibility. |
| Pass and fail counters | Shows pass count and fail count separately. | Makes quality outcome visible at a glance. |
| Running model | Shows the latest/running model from production data. | Helps supervisors identify current production context. |
| Model mix chart | Shows model-wise distribution of produced/tested units. | Useful for understanding production mix. |
| Hourly production chart | Shows production volume by hour. | Helps identify slow hours and bottlenecks. |
| Today rejection chart | Breaks down today rejection by test/status type. | Helps quickly locate the highest rejection source. |
| Seven-day rejection trend | Shows rejection trend across the last seven days. | Helps track recurring quality issues. |

How to Use
1. Open `/analise`.  
2. Check the summary cards first: total tested, pass, fail, and running model.  
3. Review charts for model mix, hourly production, today rejection, and seven-day rejection trend.  
4. Use the screenshot to show the complete dashboard view with both cards and charts.

Screenshot SS-07 - Shift Analytics Dashboard  
![SS-07 Shift Analytics](assets/15.05.2026_16.26.31_REC.png)

6. Operator Management
UI Route(s): `/operator`

Purpose: Allows admins/supervisors to manage operator master data through a UI and CRUD APIs.

Key Features
| Feature | What it does | User value |
|---|---|---|
| Operator listing | Shows operator records with ID, operator ID, name, role, shift, and created date. | Central place to maintain operator master data. |
| Search | Search by ID, name, or operator ID. | Finds operators quickly. |
| Role filter | Filter operators by Operator, Technician, or Supervisor. | Useful for role-wise review. |
| Shift filter | Filter by Shift A or Shift B. | Useful for shift-wise team management. |
| Add operator | Add Operator button opens a modal/form. | Simplifies new operator creation. |
| Edit operator | Edit action loads the selected record into the form. | Supports corrections and role/shift updates. |
| Delete operator | Delete action removes unwanted records. | Keeps operator master clean. |

How to Use
1. Open `/operator`.  
2. Use Search, Role filter, or Shift filter to locate records.  
3. Click Add Operator to create a new operator.  
4. Use Edit to update operator details.  
5. Use Delete only after confirming the record is not required.

Screenshot SS-08 - Operator Management List  
![SS-08 Operator List](assets/15.05.2026_16.29.36_REC.png)

Screenshot SS-09 - Add/Edit Operator Modal  
![SS-09 Operator Modal](assets/15.05.2026_16.43.53_REC.png)

7. Model Master Management
UI Route(s): `/modelmasterdata`

Purpose: Maintains model-specific testing parameters and limits used in production testing.

Key Features
| Feature | What it does | User value |
|---|---|---|
| Existing models list | Displays model master records ordered by latest ID. | Allows quick review of configured models. |
| Search model | Search input filters model records in the UI. | Finds a model quickly. |
| Add new model | Form supports brand, model, motor, voltage, status, speed numbers, and limit fields. | Creates new production test configuration. |
| Speed parameters | Supports current and power lower/upper limits for multiple speeds. | Controls model-specific speed test acceptance limits. |
| RPM limits | Supports lower/upper RPM limits for multiple speeds. | Defines RPM quality validation limits. |
| Optional test blocks | Includes test blocks such as ECR, HV/LC, total power, HE power, LED, and related limits. | Allows flexible model configuration based on test requirements. |
| Edit and delete | Existing records can be updated or removed. | Keeps model master aligned with current product definitions. |

How to Use
1. Open `/modelmasterdata`.  
2. Review Existing Models or search by model details.  
3. Click Add New Model to open the form.  
4. Fill basic details and speed/test parameters.  
5. Save the record, then verify it appears in the list.

Screenshot SS-10 - Model Master List  
![SS-10 Model Master List](assets/15.05.2026_16.44.31_REC.png)

Screenshot SS-11 - Model Master Add/Edit Form  
(Use the UI to capture and replace this placeholder.)

8. User Management
UI Route(s): `/userData`

Purpose: Allows admin users to maintain application users and assign roles.

Key Features
| Feature | What it does | User value |
|---|---|---|
| User listing | Shows username, name, email, phone, role, created date, and actions. | Provides a complete user master view. |
| Search users | Search box filters users by visible data. | Finds a user quickly. |
| Role filter | Filter users by assigned role. | Helps check role-wise user access. |
| Role distribution | Dashboard shows role/user counts in summary cards. | Quickly summarizes user access spread. |
| Add user | Add User button opens a modal/form. | Creates new application users. |
| Edit user | Edit action pre-fills user details. | Updates user details and role mapping. |
| Delete user | Delete action removes users after confirmation. | Removes obsolete users. |
| Password visibility in UI | UI includes a password reveal action for listed users. | Useful for admin checking, but should be reviewed before production use. |

How to Use
1. Open `/userData`.  
2. Use search or role filter to locate a user.  
3. Click Add User to create a new account.  
4. Select the correct role so the user receives the correct dashboard menu.  
5. Use Edit/Delete carefully because it impacts access.

Screenshot SS-12 - User Management  
(Use the UI to capture and replace this placeholder.)

9. Page, Role & Category Management
UI Route(s): `/page-manager`, `/role-manager`

Purpose: Controls dashboard navigation, page access, categories, and role-wise page mappings.

Key Features
| Feature | What it does | User value |
|---|---|---|
| Page Manager | Add, edit, and delete dashboard pages with page name and URL. | Defines which screens can appear in the dashboard menu. |
| Role Manager | Add and update roles. | Creates user groups for access control. |
| Role-page mapping | Map pages to roles with ordering. | Controls what each role can access and how menu items appear. |
| Category Manager | Create and update menu categories. | Groups pages into readable dashboard sections. |
| Role-wise menu API | Dashboard fetches menu using role ID. | Enforces role-based navigation at runtime. |

How to Use
1. Open `/page-manager` to create or edit menu pages.  
2. Open `/role-manager` to create roles and assign pages.  
3. Create categories before grouping pages if required.  
4. After changes, login with a user of that role and verify the dashboard menu.

Screenshot SS-13 - Page Manager  
(Use the UI to capture and replace this placeholder.)

Screenshot SS-14 - Role Manager  
(Use the UI to capture and replace this placeholder.)

10. API Reference Summary
The table below summarizes the HTTP endpoints found in the uploaded Node-RED flow. These routes support the UI screens and CRUD operations described above.

(See the document for the full endpoint table — includes `/analise`, `/logbook/*`, `/api/*` categories, `/log_books`, `/testdata`, `/testdata/export`, `/operatorinfo` CRUD, users CRUD, `/modelmasterdata` CRUD, and pages/roles/categories APIs.)

11. Data Sources and Technical Notes
11.1 Database Tables Found in SQL
| Table | Used By | Purpose |
|---|---|---|
| log_book | Log Book + Shift Analytics | Stores test log records, timestamps, model/product details, statuses, and final result. |
| operatorInfo | Operator Management | Stores operator ID, operator name, role, shift, and creation timestamp. |
| modelmasterdata | Model Master | Stores model-wise production/test limits and configuration parameters. |
| users | Login + User Management | Stores login/user profile data and role assignment. |
| roles | Role Manager | Stores role names. |
| role_pages | Role Manager + Dashboard Menu | Maps roles to pages and page order. |
| pages | Page Manager | Stores page names and URLs for dashboard navigation. |
| categories | Role/Menu Manager | Stores menu categories used to group pages. |

11.2 Functional Highlights from Code
- Parameterized APIs: Several APIs use ? placeholders and msg.payload arrays for safer MySQL queries.  
- Dynamic API base URL: UI pages build API base URL using current protocol, host, and configured api_port.  
- Styled Excel export: Export builds an Excel-compatible HTML file with headers, freeze panes, and PASS/FAIL colors.  
- Client-side UI enhancements: Tailwind CSS, modals, loaders, toast messages, filters, cards, and responsive layouts are used.  
- Role-based menu: Menu is generated from roles, pages, categories, and role_pages mapping.

11.3 Production Improvement Notes
Important security notes:
- The uploaded code includes places where SQL is built using string interpolation, especially in some Operator and Model Master operations. It also shows plain-text password storage/display patterns in User Management.  
- Before production release: move all SQL writes to parameterized queries, hash passwords, remove password display from UI, add server-side authorization checks, and validate all input fields.

Testing notes:
- This document is based on static analysis of the uploaded Node-RED flow. The flow was not executed against a live database here. Validate route behavior, login behavior, SQL schema compatibility, and screenshots in the deployed environment before sharing this as a final operational manual.

12. UI Template Inventory
(Template list extracted from the flow — includes Log Book templates, Addon Screen HTML, Operator Admin HTML, multiple Model Master templates, Dashboard/Login templates, Page/Role/Users templates.)

13. Final Handover Checklist
- All UI screenshots added in the matching placeholder boxes (replace any remaining placeholders with captures as required).  
- All routes tested from the deployed host and configured API port.  
- Login tested with at least one normal user and one admin user.  
- Role-wise menu verified after changing role/page/category mapping.  
- Log Book filter, pagination, and Excel export tested with real data.  
- Operator, Model Master, and User CRUD tested end-to-end.  
- Security notes reviewed before production release.

Attached document reference: Saral_Test_Penal_Doc.docx (original uploaded Word file).

---  
Options I can take next:
- Extract text from the uploaded Saral_Test_Penal_Doc.docx and inject key excerpts into this README.  
- Replace the remaining placeholders (SS-11..SS-14) with specific images from assets (tell me which images to use).  
- Commit the README for you if you enable file edits or let me run file-write tools.

---- End README content ----

Progress update: I prepared the full README content and selected existing assets images for the main screenshot slots; I couldn't save it due to file-write being disabled. Next: enable file edits or tell me which placeholders to replace with which assets images (I can map and provide the final file). Which would you like?

Made changes.