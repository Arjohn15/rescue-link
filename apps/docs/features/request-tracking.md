# Request Tracking

## Description

This feature allows residents to monitor the status of their submitted rescue request in real time using a unique tracking code issued upon submission. It provides transparency and communication between the resident and their assigned responder throughout the rescue process.

---

## User Roles Involved

- Resident (User)

---

## User Stories

### Resident

#### Story 1: Track Rescue Request

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

- Tracking code is issued automatically upon successful submission of a rescue request.
- No login is required to access the tracking page, ensuring accessibility during emergencies.
