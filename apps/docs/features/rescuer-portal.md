# Rescuer Portal

## Description

This feature provides rescue team leads and members with a dedicated portal to view their assigned rescue requests and manage deployment activities. The portal is accessible at /rescuer/login and is separate from the admin dashboard. The team lead's device broadcasts real-time GPS location during an active deployment, enabling residents and admins to track the team's movement on the map.

---

## User Roles Involved

- Rescue Team Lead
- Rescue Team Member

---

## User Stories

### Rescue Team Lead

#### Story 0: Accept Invite and Set Password

As a rescue team lead,
I want to accept my invitation and set my own password,
so that I can securely access the rescuer portal.

**Acceptance Criteria**

- [ ] Team lead receives an invite email with a unique registration link
- [ ] Link directs the team lead to a password setup page
- [ ] Team lead can input and confirm their password
- [ ] System validates that the password meets minimum requirements
- [ ] Account is activated upon successful password setup
- [ ] Team lead is redirected to the rescuer login page after activation
- [ ] System displays an error if the invite link is expired or invalid
- [ ] Invite link can only be used once

---

#### Story 1: Log In and Out as Rescue Team Lead

As a rescue team lead,
I want to log in and out of the rescuer portal with my credentials,
so that I can access my team's assignments and manage deployments.

**Acceptance Criteria**

- [ ] Team lead can access a dedicated login page at /rescuer/login
- [ ] Login form includes an email and password field
- [ ] System authenticates credentials against the database
- [ ] Team lead is redirected to the rescuer portal upon successful login
- [ ] System displays an error message for invalid credentials
- [ ] Session expires after a period of inactivity
- [ ] Team lead can manually log out from the portal
- [ ] System clears the session and redirects to the login page upon logout

---

#### Story 2: View Assigned Rescue Request

As a rescue team lead,
I want to view the details of our assigned rescue request,
so that my team knows where to go and what kind of assistance is needed.

**Acceptance Criteria**

- [ ] Portal displays the currently assigned rescue request
- [ ] Request details include resident name, contact number, location, detected needs with icons, and message
- [ ] Request location is shown on a map
- [ ] Portal displays an appropriate message if no request is currently assigned
- [ ] Details update in real time when a new request is assigned

---

#### Story 3: Broadcast Location During Deployment

As a rescue team lead,
I want my location to be broadcast automatically when our team is deployed,
so that the resident and admin can track our movement in real time.

**Acceptance Criteria**

- [ ] Location broadcasting starts automatically when the team status changes to deployed
- [ ] Team lead's device GPS is used as the location source
- [ ] Location updates are sent in real time via Socket.IO
- [ ] Admin dashboard map reflects the team lead's moving location
- [ ] Resident tracking page reflects the team lead's moving location
- [ ] A route path is drawn from the team lead's current location to the resident's location
- [ ] Location broadcasting stops automatically when the request is marked as completed
- [ ] System requests GPS permission from the device if not already granted
- [ ] System displays an appropriate message if GPS permission is denied

---

#### Story 4: Communicate with Resident

As a rescue team lead,
I want to send and receive messages with the resident from the request details view,
so that I can provide on-ground updates and coordinate directly with them during the rescue.

**Acceptance Criteria**

- [ ] Team lead can send messages to the resident from the request details view
- [ ] Team lead can view the full message history including messages from the resident and the admin
- [ ] Each message is labeled with the sender role (Admin, Team Lead, Resident)
- [ ] Chat updates in real time without needing to refresh the page
- [ ] Team lead can only chat with residents of requests assigned to their team

---

#### Story 5: Update Request Status

As a rescue team lead,
I want to update the status of the assigned rescue request,
so that the admin and resident are informed of the deployment progress.

**Acceptance Criteria**

- [ ] Team lead can update the request status to In Progress or Completed
- [ ] Status change is reflected in real time on the admin dashboard and resident tracking page
- [ ] Location broadcasting stops automatically when status is set to Completed
- [ ] Team status changes back to available when the request is marked as Completed

---

### Rescue Team Member

#### Story 6: Accept Invite and Set Password

As a rescue team member,
I want to accept my invitation and set my own password,
so that I can securely access the rescuer portal.

**Acceptance Criteria**

- [ ] Team member receives an invite email with a unique registration link
- [ ] Link directs the team member to a password setup page
- [ ] Team member can input and confirm their password
- [ ] System validates that the password meets minimum requirements
- [ ] Account is activated upon successful password setup
- [ ] Team member is redirected to the rescuer login page after activation
- [ ] System displays an error if the invite link is expired or invalid
- [ ] Invite link can only be used once

---

#### Story 7: Log In and Out as Rescue Team Member

As a rescue team member,
I want to log in and out of the rescuer portal with my credentials,
so that I can view my team's current assignment.

**Acceptance Criteria**

- [ ] Team member can access the same login page at /rescuer/login
- [ ] Login form includes an email and password field
- [ ] System authenticates credentials and identifies the member's role
- [ ] Team member is redirected to a read-only assignment view upon login
- [ ] System displays an error message for invalid credentials
- [ ] Session expires after a period of inactivity
- [ ] Team member can manually log out from the portal
- [ ] System clears the session and redirects to the login page upon logout

---

#### Story 8: View Team Assignment

As a rescue team member,
I want to view the details of our team's current assignment,
so that I am informed about where we are going and what assistance is needed.

**Acceptance Criteria**

- [ ] Portal displays the currently assigned rescue request in read-only mode
- [ ] Request details include resident location, detected needs with icons, and message
- [ ] Request location is shown on a map
- [ ] Member cannot update request status or broadcast location
- [ ] Member cannot send messages to the resident or admin
- [ ] Portal displays an appropriate message if no request is currently assigned
- [ ] Details update in real time when a new request is assigned

---

## Notes (Optional)

- Rescue team accounts are created solely by admins. No self-registration is available.
- Only the team lead's device broadcasts GPS location during deployment.
- Team members have read-only access to assignment details and cannot modify request status or participate in chat.
- The rescuer portal is a separate login interface from the admin dashboard.
- Story 0 and Story 6 share the same password setup page since the flow is identical for both leads and members.
