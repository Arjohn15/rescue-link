# Rescue Request Submission

## Description

This feature allows residents to submit rescue requests during emergency situations. It collects essential information such as personal details, location, and specific needs to enable responders to provide appropriate assistance efficiently.

---

## User Roles Involved

- Resident (User)

---

## User Stories

### Resident

#### Story 1: Submit Rescue Request

As a resident,
I want to submit a rescue request,
so that I can ask for help during emergencies.

**Acceptance Criteria**

- [ ] User can input full name
- [ ] User can input contact number
- [ ] User must provide location (can input or select location on the map)
- [ ] User can select at least one need (e.g., food, water, medical)
- [ ] User can enter an optional message
- [ ] User can upload images or short videos
- [ ] System enforces file type and size limits for uploaded media
- [ ] Form validates required fields before submission
- [ ] System displays a confirmation message with the unique tracking code after submission

---

#### Story 2: Track Request

As a resident,
I want to track the status of my submitted rescue request,
so that I know whether help is on the way and what to expect next.

**Acceptance Criteria**

- [ ] User can enter their tracking code on a tracking page to view their request
- [ ] Tracking page displays the current status of the request (Pending, Responding, In Progress, Completed)
- [ ] User receives real-time status updates without needing to refresh the page
- [ ] User is notified (on-screen alert) when their request status changes
- [ ] Tracking page displays an estimated response time (if available)
- [ ] User can view any messages sent by the responder via the chat interface
- [ ] User can send messages to the admin/responder through the chat interface on the tracking page
- [ ] System displays an error message if the entered tracking code is invalid or not found
- [ ] No login is required — access is granted through the unique tracking code only or clicking the floating status bar in the browser where the request was made

---

## Notes (Optional)

- No user login is required to submit a request to ensure accessibility during emergencies.
