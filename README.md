<p align="center">
  <img src="assets/icon.png" alt="Jamf Extender" width="128">
</p>

# Jamf Extender

A Chrome extension that enhances the Jamf Pro web interface with inline smart group membership, configuration profile deployment status, policy scope details, and smart group usage data without navigating away from the page.

[Install for Safari from Mac App Store](https://chromewebstore.google.com/detail/jamf-extender/gjgdljckajmddkbcodiaimlfhjalfdnk)

[Install for Firefox from Firefox Browser Add-ons](https://addons.mozilla.org/en-US/firefox/addon/jamf-extender/)

[Install for Chrome and Edge from Chrome Web Store](https://chromewebstore.google.com/detail/jamf-extender/gjgdljckajmddkbcodiaimlfhjalfdnk) (For Edge, enable "Allow extensions from other stores" then navigate to this link.)

## What's new in 1.2.0

### macOS Keychain Integration

Jamf Pro API credentials are now stored encrypted in the macOS Keychain instead of unencrypted browser storage. The **Jamf Extender Safari app** acts as a Keychain gateway for all browsers — credentials saved in any browser are shared via a single Keychain entry. Safari uses Keychain automatically; Chrome, Edge, and Firefox can opt in by opening the Safari app and clicking **"Set Up Other Browsers"**. Existing credentials are migrated automatically on first launch. Falls back to browser storage if the Safari app is not installed.

### API Role Privilege Picker

Jamf Pro's native Privileges combobox is a slow, text-only search across hundreds of entries. Jamf Extender injects a **Privilege Picker** panel directly beneath the Privileges field on any API role edit page:

- **Resource-grouped CRUD grid** — privileges are parsed into rows per resource (e.g., "Computers", "Smart Computer Groups") with C/R/U/D checkboxes in columns. Disabled cells show actions that don't exist for a given resource.
- **Category filters** — quick pills for All / Selected / Create / Read / Update / Delete / Other to narrow the list at a glance.
- **Paste CSV to filter** — paste a comma- or newline-separated list of privilege names into the search box and the picker filters (and highlights) exactly those entries. Click **"Select From Input"** to toggle them all on in one shot — great for replicating a role from another instance or applying a known-good privilege set.
- **Select All Visible / Deselect All** — bulk operate on whatever's currently filtered.
- **Save Role** — writes the selected privileges back to Jamf Pro via the Pro API (POST for new roles, PUT for existing) and, for new roles, redirects to the edit URL so you can keep iterating.

### Mobile Device Apps — XML Validator

A live XML/Plist syntax assistant on the mobile device app detail page (`mobileDeviceApps.html?id={id}`). As you type in the AppConfig plist textarea, the assistant checks for bracket/quote balance, malformed tags, unescaped `&` characters, tag matching, plist structure (key/value alternation in `<dict>`), leading/trailing whitespace, and empty values. A line-number gutter is injected alongside the textarea. Issues show severity, line number, and a suggested fix. Runs entirely client-side — no credentials required.

### Change Management Logs — Advanced Filtering

An **Advanced Filtering** bar is injected above the Change Management Logs table with per-column filters: free-text inputs for Username, Object Type, and Object Name, plus an Action dropdown (Created, Updated, Deleted, etc.). Filters apply live as you type and combine with AND logic. A row counter shows how many entries match, and a Clear button resets all filters in one click — much faster than scrolling or re-sorting the native table.

### Dashboard Health Status

An inline **Health** indicator appears next to the Jamf Pro version on the dashboard. It calls Jamf Pro's built-in `GET /api/v1/health-status` endpoint (authenticated with your API client) and reads the **`oneMinute`** metric for each reported health group — a ratio from 0.0 to 1.0 representing the rolling success rate of Jamf Pro's internal health probes for that subsystem over the last 60 seconds.

- **"Healthy"** (green) — every group's `oneMinute` ratio is 1.0 (100%).
- **"{N} degraded"** (yellow) — N groups are currently below 100%, meaning some internal probes failed in the last minute.
- **"Unavailable"** / **"Unknown"** — the endpoint didn't respond or returned no recognizable metrics.

Note: this surfaces Jamf Pro's own internal probe ratios — it is not a holistic environment check (no cert expiry, disk space, backup status, APNs latency, or patch feed freshness).

## Features

### Smart Computer Groups

- **Device Table** — View all devices in a smart group directly on the detail page, with serial numbers and pagination
- **Hover Popovers** — Hover over group names or counts on the list page to preview device membership
- **"Where It's Used" Drawers** — See which policies, profiles, restricted software, Mac apps, patch policies, eBooks, app installers, webhooks, and blueprints target, limit, or exclude each smart group
- **Actions** — Convert unused smart groups to advanced computer searches, or delete them directly
- **Email Notification Indicator** — See which smart groups have "Notify on change" enabled
- **Nesting Detection** — Caution/warning badges for groups with nested "Computer Group" criteria

### Mobile Device Smart Groups

- **Device Table** — View mobile devices in a group on the detail page (name, serial, Wi-Fi MAC)
- **Hover Popovers** — Hover over group names/counts to preview mobile device membership
- **"Where It's Used" Drawers** — See which mobile config profiles, mobile device apps, eBooks, webhooks, and blueprints scope to each group
- **Actions** — Convert to advanced mobile device search, or delete directly
- **Email Notification Indicator** — See which mobile smart groups have notifications enabled
- **Nesting Detection** — Caution/warning badges for groups with nested "Mobile Device Group" criteria

### Configuration Profiles (macOS + iOS)

- **Payloads Drawer** — Click the "Payloads" button next to any profile name to expand an inline drawer showing all configured payloads (Wi-Fi, VPN, Restrictions, Certificates, SCEP, PPPC, etc.) with human-friendly labels and raw `PayloadType` identifiers
- **Compare Profiles** — Click the "Compare Profiles" button above the table to enter selection mode. Check 2 or more profiles and click "Compare" to see a side-by-side comparison modal showing each profile's scope summary and payload matrix. Shared payloads are highlighted green; unique payloads are easy to spot at a glance
- **Scope Hover Popovers** — Hover over the Scope column to see full scope details: targeted groups with device membership, individual devices, limitations, exclusions, and "All Computers"/"All Mobile Devices" flags
- **Deployment Status Popovers** — Hover over Completed/Pending/Failed cells to see deployment details with device names and serial numbers

### Policies

- **Scope Hover Popovers** — Hover over the Scope column on the policies list to see full scope details: targeted groups, individual computers, "All Computers" flag, limitations, and exclusions — all with clickable links
- **Policy Risk Report** — Scan bar above the policies table flags risky policies:
  - **Caution** — Ongoing frequency + Recurring Check-in trigger (scoped to specific groups/computers)
  - **Warning** — Ongoing frequency + Recurring Check-in trigger + All Computers scope

### Blueprints

- **Settings Hover Popover** — Hover over a blueprint name or "Open" button on the blueprints list page to see all configured settings (DDM profiles, passcode policies, software update settings, and more) with human-readable setting names and color-coded values.
- **Scope Hover Popover** — Hover over "Scoped to" on any blueprint card to see the resolved scope: targeted smart groups with device membership, computer names, and serial numbers. Includes an **Export CSV** button for full device-level scope reports.

### Licensed Software (Application Usage)

- **Application Usage Report** — On the Licensed Software list page (`licensedSoftware.html`), a panel above the table lets you select an Advanced Computer Search, pick a time range (1 day, 7 days, 30 days, 3 months, 1 year, or custom dates), and run an aggregated application usage report across all computers in that search. Results show each application's total foreground time, total opens, and number of distinct computers — sortable by any column and filterable by app name. The selected search is saved per Jamf Pro instance.
- **Unused App Search** — A second tab on the same panel. Enter an application name (case-insensitive partial match), select an Advanced Computer Search and time range, then click "Find Devices" to discover all computers that have NOT used that application in the specified period. Results show Computer Name (linked to device record), Serial Number, Username, Email, Model, and Computer ID with a summary count (e.g., "12 of 50 devices have not used Self Service in the last 30 days"). Export results as CSV (named after the search) via the "Export CSV" button. Save your search configuration (search + app name + time range) with "Save Search" and reload it later from the "Saved Searches" dropdown.

### Mobile Device Apps — XML Validator

- **Live Syntax Assistant** — On the mobile device app detail/edit page (`mobileDeviceApps.html?id={id}`), a validation panel appears below the AppConfig plist textarea. As you type, the assistant checks bracket/quote balance, malformed tags, unescaped `&` characters, tag matching, plist structure (key/value alternation in `<dict>`), leading/trailing whitespace in values, and empty/blank elements.
- **Line Number Gutter** — A line-number gutter is injected alongside the textarea for quick reference when issues point at specific lines.
- **Issue Cards** — Problems are listed with severity badges, line numbers, and suggested fixes. Runs entirely client-side — no credentials required.

### Change Management Logs

- **Advanced Filtering Bar** — On the Change Management Logs page, a filter bar is injected above the native table with free-text inputs for Username, Object Type, and Object Name, plus an Action dropdown. Filters apply live and combine with AND logic.
- **Row Counter + Clear** — A live count of matching rows updates as you type. A Clear button resets all filters in a single click.

### Dashboard Health

- **Inline Health Indicator** — On the Jamf Pro dashboard, a **"Health:"** status appears next to the version string in the footer/aside area. Polls Jamf Pro's `GET /api/v1/health-status` endpoint and reads the `oneMinute` probe-success ratio per health group. Shows **"Healthy"** (all groups at 100% over the last minute), **"{N} degraded"** (one or more groups below 100%), or **"Unavailable"** / **"Unknown"** if the endpoint doesn't respond.
- **Expandable Breakdown** — Click the chevron to reveal a per-group list with color-coded percentages (green for 100%, yellow for <100%) so you can pinpoint which subsystem is affected without leaving the dashboard. This reflects Jamf Pro's own internal service probes — it does not cover certificate expiry, disk space, backup state, APNs push latency, or patch feed freshness.

### Jamf Security Cloud (Security Cloud)

- **Quick Setup** — Detects `radar.wandera.com` tabs and generates API client keys automatically using your active session cookies. Credentials are stored per customer ID to support multiple Security Cloud instances.
- **Status Pill** — The persistent status pill appears on `radar.wandera.com` pages anchored next to the Jamf nav logo, showing configure/active state based on stored Security Cloud credentials for the current customer ID
- **Portal Groups** — Link a Jamf Pro instance to a Security Cloud customer ID and/or a Jamf Protect origin (e.g., "Dev" = dev.jamfcloud.com + Security Cloud customer 12345 + overview.protect.jamfcloud.com). Configured in the extension popup under **Portal Groups**.
- **Panel Toggle** — The Security Cloud side panel on device detail pages can be hidden/shown via the "Security Cloud panel" toggle in Preferences. Defaults to on.
- **Security Cloud Device Lookup** — When viewing a mobile device in Jamf Pro (`mobileDevices.html?id={id}`), if a portal group exists for that instance, a "Security Cloud" toggle button appears next to the History button in the breadcrumb actions bar. Clicking it opens a collapsible panel below the breadcrumb bar showing risk category, status, enrollment, last connector update, last DNS traffic, and last private access DNS traffic. Data is fetched lazily on first open. The panel header links directly to the device in Security Cloud.
- **Vulnerability Management** — The Security Cloud side panel shows a "Vulnerabilities" section with per-software breakdown listing each app/OS with version, vulnerability count, and max severity badge. Each software entry shows its vulnerability count with an expandable drawer listing individual CVEs (severity badge, exploited flag, linked to NVD). The section title links to the vulnerability management page in Security Cloud. Available on both **computer** and **mobile device** detail pages.
- **App Risk Ratings** — On the mobile device apps list page (`mobileDeviceApps.html`), color-coded risk badges (HIGH/MEDIUM/LOW) appear inline next to each app name. Cross-references Jamf Pro app bundle IDs with Security Cloud app-insights data. Requires a Portal Group linking the Jamf Pro instance to a Security Cloud customer ID.

### Jamf Protect

- **Quick Setup** — Detects `*.protect.jamfcloud.com` tabs and creates a read-only API client automatically using the Auth0 JWT captured from your browser session. Credentials are stored per origin.
- **Computer Detail Panel** — When viewing a computer in Jamf Pro (`computers.html?id={id}`), if Protect credentials are configured, a "Jamf Protect" section appears at the top of the security side panel showing connection status (color-coded), last connection time, plan name, Protect agent version, and an alert summary grid showing High, Medium, and Low severity rows (color-coded: red, orange, yellow) with New and In Progress counts bucketed by 24h, 7d, and 30d time periods. The extension matches the computer by UDID across all configured Protect instances.
- **Panel Toggle** — The Jamf Protect section on device detail pages can be hidden/shown via the "Jamf Protect panel" toggle in Preferences. Defaults to on.

### Mass Update Tool (MUT)

- **CSV-Based Bulk Updates** — Upload a CSV file to update Jamf Pro inventory records in bulk, inspired by the [MUT](https://github.com/jamf/mut) macOS app. Accessible via the "MUT" button in both the extension popup and the sidebar settings panel when on a Jamf Pro page.
- **Attribute Updates** — Update fields on Computers (23 fields + extension attributes), Mobile Devices (22 fields + EAs), and Users (9 fields + EAs). Blank cells are skipped; `CLEAR!` clears the value.
- **Scope Updates** — Add, remove, or replace members of Computer Static Groups, Mobile Device Static Groups, User Static Groups, Computer Prestage Enrollments, and Mobile Device Prestage Enrollments using a single-column CSV of identifiers.
- **Template Downloads** — Download pre-formatted CSV templates for all 8 update types directly from the modal. Attribute templates (Computers, Mobile Devices, Users) offer two options when clicked: "Pre-filled" fetches all identifiers from your Jamf Pro instance and downloads a CSV with the first column populated with real serial numbers or usernames; "Blank" downloads an empty template. Remaining columns are left blank so only the fields you fill in will be updated.
- **Record Preview** — After uploading a CSV, browse through records one at a time to verify values before submitting. Fields show "(unchanged)" for blank cells and "will be cleared" for `CLEAR!` values.
- **Background Processing** — Records are processed sequentially in the background service worker with per-record progress tracking. The popup can be closed and reopened without losing progress.
- **Cancel & Resume** — Cancel a running batch at any time. Reopen the popup to see current progress or results.
- **Results & Log** — After completion, see succeeded/failed counts with a scrollable error list. Download a CSV log of all results.

### API Roles & Clients

- **Privilege Picker** — On any API role edit page, a panel appears beneath the native Privileges combobox with a resource-grouped CRUD grid, category filter pills (All / Selected / Create / Read / Update / Delete / Other), text search, and **paste-to-filter** support (paste a CSV/newline-separated list of privilege names to instantly filter and highlight only those).
- **Bulk Operations** — "Select All Visible" toggles everything currently filtered; "Select From Input" selects only the entries matched by a pasted CSV; "Deselect All" clears the entire role.
- **Save Role** — Saves selected privileges directly via the Jamf Pro API (POST for new roles, PUT for existing) without needing to click through the native save flow. New roles auto-redirect to their edit URL after creation.

### Custom Jamf Pro Domains (Self-Hosted)

- **Self-Hosted Support** — Works out of the box with `*.jamfcloud.com` instances, and supports self-hosted or custom domain Jamf Pro instances (e.g., `jss.company.com:8443`, `jamfpro.mycompany.com`) after a one-time access grant from the extension popup.
- **Persistent Access** — The access grant is permanent across browser restarts. All features work identically on custom domains.
- **Per-Instance Management** — Add/remove custom domains from the extension popup. The sidebar panel (inside Jamf Pro) shows the list and supports deletion.

### macOS Keychain Integration

- **Encrypted Credential Storage** — Jamf Pro API credentials are stored in the macOS Keychain instead of unencrypted browser storage.
- **Safari App Gateway** — The Jamf Extender Safari app acts as the Keychain gateway for all browsers. Safari uses Keychain automatically; Chrome, Edge, and Firefox opt in via "Set Up Other Browsers" in the Safari app.
- **Cross-Browser Sharing** — Credentials saved in any browser are immediately available in all other browsers through a shared Keychain entry (service `com.jamf.jex`, account = Jamf Pro origin).
- **Automatic Migration** — Existing credentials in browser storage are migrated to Keychain on first launch after the helper becomes available. Falls back to browser storage if the Safari app is not installed.

### General

- **Persistent Status Pill** — Always-visible indicator showing extension state (awaiting login, needs configuration, credentials expired, permissions needed, or active). Appears on both Jamf Pro and Security Cloud pages.
- **Nav Bar Shortcuts** — Bookmark any Jamf Pro page as a quick-link shortcut button in the top navigation bar. Click the "+" button in the shortcut bar (or "Add Current Page" in the sidebar Shortcuts section) to add the current page. Shortcuts appear as pill-shaped buttons centered in the top nav bar, with active-page highlighting. Shortcuts are stored per Jamf Pro instance, so each server (e.g. overview.jamfcloud.com vs trial.jamfcloud.com) has its own independent set. Manage shortcuts (edit, reorder via drag-and-drop, delete) in the sidebar panel's Shortcuts section.
- **Sidebar Settings Panel** — A "Jamf Extender" button in the Jamf Pro sidebar navigation (above "Resource Center") opens a flyout panel with the same settings as the toolbar popup: MUT button (opens full Mass Update Tool overlay inline), Preferences (shimmer, hide dashboard setup cards, Security Cloud panel toggle, Jamf Protect panel toggle, nav bar color), Shortcuts (list, reorder, edit, delete, add current page), Portal Groups (add/delete, with optional Security Cloud and Protect linking), and Configurations (Quick Setup, manual credential entry). Closes on re-click, click outside, Escape, or page navigation. Panel repositions when the sidebar collapses/expands.
- **Pre-scan Permission Checks** — Scans are blocked entirely if the API client is missing required permissions, with a clear message and link to re-run Quick Setup
- **Nav Bar Color** — Customize the Jamf Pro top navigation bar color via a color picker in the extension popup under **Preferences**. Changes apply live without page reload. Click "Reset" to restore the default.
- **Shimmer Underlines** — Animated gradient underlines on all hoverable/interactive elements (group names, counts, scope cells, profile status links) to signal interactivity. Can be toggled on/off in the extension popup under **Preferences**.
- **Hide Dashboard Setup Cards** — Hides the Setup Tasks container on the Jamf Pro dashboard. Can be toggled on/off in the extension popup under **Preferences**.
- **Auto-Scan Group Usage** — By default, smart group usage scanning requires clicking a "Scan for Smart Group Usage" button on the smart groups list page. Enable "Auto-scan group usage on load" in **Preferences** to automatically scan when the page loads (previous behavior).

### Portal Groups Setup

Portal groups link a Jamf Pro instance to a Security Cloud customer ID and/or a Jamf Protect origin, enabling cross-platform device lookups:

1. Set up at least one **Jamf Pro** instance (via Quick Setup or Manual Setup)
2. Set up at least one **Security Cloud** instance and/or one **Jamf Protect** instance
3. Click the **Jamf Extender** icon in your browser toolbar
4. In the **Portal Groups** section, enter a group name (e.g., "Dev" or "Prod")
5. Select the Jamf Pro instance from the dropdown
6. Optionally select a Security Cloud customer ID and/or a Jamf Protect instance (at least one is required)
7. Click **Add Group**

Now, when you view a device detail page in that Jamf Pro instance, the Security Cloud and/or Protect panels will appear automatically.

### Manual Setup

1. In Jamf Pro, go to **Settings > API Roles and Clients**
2. Create an API role with these privileges:
   - Read Computers
   - Read Smart Computer Groups
   - Read Static Computer Groups
   - Read Policies
   - Read macOS Configuration Profiles
   - Read Restricted Software
   - Read Mac Applications
   - Read Patch Policies
   - Read eBooks
   - Read Webhooks
   - Read Mobile Devices
   - Read Smart Mobile Device Groups
   - Read Static Mobile Device Groups
   - Read iOS Configuration Profiles
   - Read Mobile Device Applications

   > **Note:** Blueprint scanning uses the browser session's Auth0 JWT (Platform API), not the API client — no blueprint-specific permissions are needed on the API role.

3. Create an API client assigned to that role
4. Click the Jamf Extender icon, expand the **Configurations** section, then the **Manual** subsection, select "Add new…" from the Portal dropdown, choose "Jamf Pro" as the type, and enter your Jamf Pro URL, Client ID, and Client Secret (or choose "Security Cloud" to enter JSC credentials, or "Jamf Protect" to enter Protect credentials)

## Usage

Once configured, navigate to any of these Jamf Pro pages:

| Page                              | What you get                                                                                                                                                                      |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Smart Computer Groups list        | Hover popovers on names/counts, "Where it's used" drawer per group, scan status bar with nesting badges                                                                           |
| Smart Computer Group detail       | Inline device table with serial numbers below the group info                                                                                                                      |
| Smart Mobile Device Groups list   | Same as computer groups but for mobile devices — popovers, drawers, scan bar                                                                                                      |
| Smart Mobile Device Group detail  | Inline device table with serial, Wi-Fi MAC below the group info                                                                                                                   |
| macOS Configuration Profiles list | Payloads drawer per profile, Compare Profiles side-by-side modal, scope hover with group membership, Completed/Pending/Failed status popovers                                     |
| iOS Configuration Profiles list   | Payloads drawer per profile, Compare Profiles side-by-side modal, scope hover with group membership, Completed/Pending/Failed status popovers                                     |
| Policies list                     | Hover popovers on Scope column, risk report bar for Ongoing + Recurring Check-in policies                                                                                         |
| Blueprints list                   | Hover popovers on blueprint cards — settings breakdown (DDM, passcode, software update, etc.) and resolved scope with device-level CSV export                                     |
| Licensed Software list            | Application Usage Report + Unused App Search with time-range selector, advanced computer search picker, saved searches, and CSV export                                            |
| Change Management Logs            | Advanced filter bar (Username, Object Type, Object Name, Action) with live row count and Clear                                                                                    |
| Mobile device apps list           | Security Cloud app risk badges (HIGH/MEDIUM/LOW) inline next to each app name (requires Portal Group)                                                                             |
| Mobile device app detail          | Live XML/Plist validator on the AppConfig editor — bracket/tag/plist checks, line-number gutter, inline issue list (no credentials required)                                      |
| API Role edit                     | Privilege Picker panel — resource-grouped CRUD grid, category filters, paste-CSV to filter, bulk select, direct save via API                                                      |
| Dashboard                         | Inline "Health:" status next to the Jamf Pro version — expandable per-group `oneMinute` probe-success ratios                                                                      |
| Computer detail                   | Security Cloud side panel (requires Portal Group) + Jamf Protect status section (requires Protect credentials)                                                                    |
| Mobile device detail              | Security Cloud side panel (risk, status, enrollment, last update, DNS traffic, threats, vulnerability per-software breakdown) with link to Security Cloud (requires Portal Group) |

The **status pill** next to the Jamf Pro logo shows the current extension state (not shown on `*.protect.jamfcloud.com` pages):

- **Gray** "Awaiting Login" — on the login page
- **Yellow** "Configure Extension" — logged in but no credentials for this instance (click for instructions)
- **Red** "Re-run Quick Setup" — credentials expired or invalid (click for instructions)
- **Yellow** "Update Permissions" — credentials work but some resource types returned 403 (click for the permissions modal)
- **Blue** "Update Available" — a newer version exists on GitHub (click for update instructions)
- **Green** "Extension Active" — everything working
