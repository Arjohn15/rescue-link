# Live Updates & Emergency Information

## Description

This feature allows administrators to post and manage emergency-related information visible to residents during disaster situations. It serves as the official communication channel for broadcasting announcements, hotlines, and resources to the public in real time.

---

## User Roles Involved

- Admin
- Resident (User)

---

## User Stories

### Admin

#### Story 1: Post Emergency Announcement

As an admin,
I want to post emergency announcements,
so that residents are informed about the current situation and what actions to take.

**Acceptance Criteria**

- [ ] Admin can create a new announcement with a title and message body
- [ ] Admin can set the announcement as active or inactive
- [ ] Active announcements are immediately visible to residents
- [ ] Admin can edit an existing announcement
- [ ] Admin can delete an existing announcement
- [ ] System displays the date and time the announcement was posted
- [ ] Announcements are ordered by most recent first

---

#### Story 2: Manage Emergency Hotlines

As an admin,
I want to manage a list of emergency hotlines,
so that residents can quickly contact the appropriate authorities during a disaster.

**Acceptance Criteria**

- [ ] Admin can add a new hotline entry with a name and contact number
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

- [ ] Admin can add a new resource entry with a title, description, and optional location
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
I want to view emergency announcements, hotlines, and resources,
so that I can stay informed and know who to contact during a disaster.

**Acceptance Criteria**

- [ ] Resident can view all active emergency announcements
- [ ] Resident can view the list of emergency hotlines
- [ ] Resident can view available rescue resources
- [ ] Resident can view the live stream if one is currently active
- [ ] Page does not require login to access
- [ ] Information updates in real time without needing to refresh the page

---

## Notes (Optional)

- This feature is publicly accessible — no login is required for residents to view emergency information.
- Admins must be logged in to post, edit, or delete any content on this page.
- Future enhancement: push notifications to alert residents when a new announcement is posted.
