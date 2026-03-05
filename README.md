<p align="center">
  <img src="assets/icon.png" alt="Jamf Extender" width="128">
</p>

# Jamf Extender

**v1.0.2**

A Chrome extension that enhances the Jamf Pro web interface with inline smart group membership, configuration profile deployment status, policy scope details, and smart group usage data — without navigating away from the page.

[Install from Chrome Web Store](https://chromewebstore.google.com/detail/jamf-extender/gjgdljckajmddkbcodiaimlfhjalfdnk)

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

### Licensed Software (Application Usage)

- **Application Usage Report** — On the Licensed Software list page (`licensedSoftware.html`), a panel above the table lets you select an Advanced Computer Search, pick a time range (1 day, 7 days, 30 days, 3 months, 1 year, or custom dates), and run an aggregated application usage report across all computers in that search. Results show each application's total foreground time, total opens, and number of distinct computers — sortable by any column and filterable by app name. The selected search is saved per Jamf Pro instance.
- **Unused App Search** — A second tab on the same panel. Enter an application name (case-insensitive partial match), select an Advanced Computer Search and time range, then click "Find Devices" to discover all computers that have NOT used that application in the specified period. Results show Computer Name (linked to device record), Serial Number, Username, Email, Model, and Computer ID with a summary count (e.g., "12 of 50 devices have not used Self Service in the last 30 days"). Export results as CSV (named after the search) via the "Export CSV" button. Save your search configuration (search + app name + time range) with "Save Search" and reload it later from the "Saved Searches" dropdown.

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

### General

- **Persistent Status Pill** — Always-visible indicator showing extension state (awaiting login, needs configuration, credentials expired, permissions needed, or active). Appears on both Jamf Pro and Security Cloud pages.
- **Nav Bar Shortcuts** — Bookmark any Jamf Pro page as a quick-link shortcut button in the top navigation bar. Click the "+" button in the shortcut bar (or "Add Current Page" in the sidebar Shortcuts section) to add the current page. Shortcuts appear as pill-shaped buttons centered in the top nav bar, with active-page highlighting. Shortcuts are stored per Jamf Pro instance, so each server (e.g. overview.jamfcloud.com vs trial.jamfcloud.com) has its own independent set. Manage shortcuts (edit, reorder via drag-and-drop, delete) in the sidebar panel's Shortcuts section.
- **Sidebar Settings Panel** — A "Jamf Extender" button in the Jamf Pro sidebar navigation (above "Resource Center") opens a flyout panel with the same settings as the toolbar popup: MUT button (opens full Mass Update Tool overlay inline), Preferences (shimmer, hide dashboard setup cards, performance toggles, Security Cloud panel toggle, Jamf Protect panel toggle, nav bar color), Shortcuts (list, reorder, edit, delete, add current page), Portal Groups (add/delete, with optional Security Cloud and Protect linking), and Configurations (Quick Setup, manual credential entry). Closes on re-click, click outside, Escape, or page navigation. Panel repositions when the sidebar collapses/expands.
- **Pre-scan Permission Checks** — Scans are blocked entirely if the API client is missing required permissions, with a clear message and link to re-run Quick Setup
- **Nav Bar Color** — Customize the Jamf Pro top navigation bar color via a color picker in the extension popup under **Preferences**. Changes apply live without page reload. Click "Reset" to restore the default.
- **Shimmer Underlines** — Animated gradient underlines on all hoverable/interactive elements (group names, counts, scope cells, profile status links) to signal interactivity. Can be toggled on/off in the extension popup under **Preferences**.
- **Hide Dashboard Setup Cards** — Hides the Setup Tasks container on the Jamf Pro dashboard. Can be toggled on/off in the extension popup under **Preferences**.
- **Auto-Scan Group Usage** — By default, smart group usage scanning requires clicking a "Scan for Smart Group Usage" button on the smart groups list page. Enable "Auto-scan group usage on load" in **Preferences** to automatically scan when the page loads (previous behavior).
