# Live Updates & Emergency Information

## Description

This feature allows administrators to post and manage emergency-related information visible to residents during disaster situations. It serves as the official communication channel for broadcasting alerts, hotlines, resources, and live streams to the public in real time. All content is publicly accessible without login.

---

## User Roles Involved

- Admin
- Resident (User)

---

## User Stories

### Admin

#### Story 1: Post Emergency Alert

As an admin,
I want to post emergency alerts with a type,
so that residents are informed about the current situation and understand how critical it is.

**Acceptance Criteria**

- [ ] Admin can create a new alert with a title and message body
- [ ] Admin can set a type for the alert (info, advisory, critical)
- [ ] Admin can set the alert as active or inactive
- [ ] Active alerts are immediately visible to residents
- [ ] Admin can edit an existing alert
- [ ] Admin can delete an existing alert
- [ ] System displays the date and time the alert was posted
- [ ] Alerts are ordered by most recent first
- [ ] Alerts are visually distinguished by type

---

#### Story 2: Manage Emergency Hotlines

As an admin,
I want to manage a list of emergency hotlines,
so that residents can quickly contact the appropriate authorities during a disaster.

**Acceptance Criteria**

- [ ] Admin can add a new hotline entry with a name, contact number, and category
- [ ] Admin can select a category for the hotline (police, fire, medical, disaster, barangay)
- [ ] Hotline is displayed with an icon corresponding to its category
- [ ] Admin can edit an existing hotline entry
- [ ] Admin can delete a hotline entry
- [ ] Hotlines are displayed in an organized list visible to residents
- [ ] Changes to hotlines are reflected immediately on the resident-facing page

---

#### Story 3: Manage Rescue Resources

As an admin,
I want to post and manage rescue-related resources,
so that residents have access to important information such as evacuation centers and relief goods distribution.

**Acceptance Criteria**

- [ ] Admin can add a new resource entry with a title, description, category, and optional location
- [ ] Admin can select a category for the resource (evacuation center, relief goods, medical, rescue team, food and water)
- [ ] Resource is displayed with an icon corresponding to its category
- [ ] Admin can edit an existing resource entry
- [ ] Admin can delete a resource entry
- [ ] Resources are visible to residents on the public-facing page
- [ ] Changes to resources are reflected immediately

---

#### Story 4: Broadcast a Live Stream

As an admin,
I want to embed or link a live broadcast,
so that residents can watch real-time updates directly from the platform.

**Acceptance Criteria**

- [ ] Admin can input a live stream URL (e.g., YouTube, Facebook Live)
- [ ] Live stream is displayed on the resident-facing page when active
- [ ] Admin can remove or deactivate the live stream when it ends
- [ ] Page shows an appropriate message when no live stream is currently active

---

### Resident

#### Story 5: View Emergency Information

As a resident,
I want to view emergency alerts, hotlines, and resources,
so that I can stay informed and know who to contact during a disaster.

**Acceptance Criteria**

- [ ] Resident can view all active emergency alerts with their type
- [ ] Alerts are visually distinguished by type (info, advisory, critical)
- [ ] Resident can view the list of emergency hotlines with their category icons
- [ ] Resident can view available rescue resources with their category icons
- [ ] Resident can view the live stream if one is currently active
- [ ] Page shows an appropriate message when no live stream is currently active
- [ ] Page does not require login to access
- [ ] Information updates in real time without needing to refresh the page

---

## Notes (Optional)

- This feature is publicly accessible. No login is required for residents to view emergency information.
- Admins must be logged in to post, edit, or delete any content on this page.
- Future enhancement: push notifications to alert residents when a new alert is posted.
