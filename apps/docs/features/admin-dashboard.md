# Admin Dashboard

## Description

This feature provides administrators with a centralized command interface for monitoring rescue requests, viewing incident locations, and managing emergency response activities. It serves as the main operational view for coordination during disaster situations.

---

## User Roles Involved

- Admin

---

## User Stories

### Admin

#### Story 0: Log In and Out as Admin

As an admin,
I want to log in and out of the dashboard with my credentials,
so that only authorized personnel can access and manage rescue operations.

**Acceptance Criteria**

- [ ] Admin can access a dedicated login page
- [ ] Login form includes an email/username field and a password field
- [ ] Form validates that both fields are filled before submission
- [ ] System authenticates credentials against the database
- [ ] Admin is redirected to the dashboard upon successful login
- [ ] System displays an error message for invalid credentials
- [ ] System locks the account or throttles attempts after repeated failed logins
- [ ] Admin session expires after a period of inactivity
- [ ] Unauthenticated users are redirected to the login page when accessing protected routes
- [ ] Admin can manually log out from the dashboard
- [ ] System clears the session and redirects to the login page upon logout

---

#### Story 1: View Dashboard Overview

As an admin,
I want to view a summary of rescue operations,
so that I can quickly understand the current emergency situation.

**Acceptance Criteria**

- [ ] Dashboard displays summary metrics
- [ ] Summary includes total requests
- [ ] Summary includes pending requests
- [ ] Summary includes in-progress requests
- [ ] Summary includes completed requests
- [ ] Metrics update in real-time as request statuses change

---

#### Story 2: View Rescue Requests on a Map

As an admin,
I want to see rescue requests plotted on a map,
so that I can identify affected locations quickly.

**Acceptance Criteria**

- [ ] Dashboard displays a map component
- [ ] Requests with location data appear as map markers
- [ ] Markers correspond to stored request coordinates
- [ ] Clicking a marker shows basic request information
- [ ] Map updates when new requests are added or statuses change

---

#### Story 3: View and Monitor Recent Rescue Requests

As an admin,
I want to view a list of recent rescue requests with their status and urgency,
so that I can monitor newly submitted incidents and prioritize coordination effectively.

**Acceptance Criteria**

- [ ] Dashboard displays a recent requests panel or list
- [ ] Each item shows resident name
- [ ] Each item shows request status
- [ ] Each item shows selected needs
- [ ] Each item shows submission time
- [ ] Recent requests are ordered by most recent first
- [ ] Requests display visible status indicators using consistent labels or badges
- [ ] Urgent or high-priority requests are visually distinguishable from others
- [ ] Status display is consistent across all dashboard widgets

---

#### Story 4: Access Request Details from the Dashboard

As an admin,
I want to open a specific rescue request from the dashboard,
so that I can review complete information and take action.

**Acceptance Criteria**

- [ ] Admin can click a request from the list or map
- [ ] System opens the corresponding request details view
- [ ] Request details include name, contact number, location, needs, message, and status
- [ ] Selected request matches the clicked dashboard item

---

#### Story 5: Communicate with Resident

As an admin,
I want to send and receive messages with a resident from the request details view,
so that I can provide updates and gather additional information during an emergency.

**Acceptance Criteria**

- [ ] Admin can send messages to the resident from the request details view
- [ ] Admin can view the full message history with the resident
- [ ] Chat updates in real-time without needing to refresh the page
- [ ] Admin can only chat with residents of requests assigned to them

---

#### Story 6: View My Assigned Requests

As an admin,
I want to have a dedicated page for rescue requests I am handling,
so that I can focus on my own assignments without confusion when multiple admins are working simultaneously.

**Acceptance Criteria**

- [ ] Admin can navigate to a dedicated "My Requests" page
- [ ] Admin can claim an unassigned request from the main dashboard
- [ ] Page displays only the requests assigned to or claimed by the currently logged-in admin
- [ ] Each item shows resident name, status, needs, and submission time
- [ ] Admin can update the status of their assigned requests from this page
- [ ] Admin can open full request details from this page
- [ ] Requests assigned to other admins are not visible on this page
- [ ] Page reflects real-time updates when request statuses change
- [ ] If the admin has no assigned requests, an appropriate empty state message is displayed

---

## Notes (Optional)

- The dashboard is the main command center for administrators.
- The map service is used for visualization of rescue request locations.
- Future enhancement: add filtering by barangay, urgency, or request type.
- Admin accounts are provisioned directly by the system administrator
  via a seed script. No public registration is available by design.
