# Rescue Request Submission

## Description

This feature allows residents to submit rescue requests during emergency situations. It collects essential information such as personal details, location, and a message describing their situation. The message is analyzed by the automated triage algorithm to detect needs and assign a priority score. The feature is optimized for low-bandwidth environments, ensuring requests can be submitted even with minimal or intermittent connectivity.

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
- [ ] User must enter a message describing their situation
- [ ] User can upload images or short videos
- [ ] System enforces file type and size limits for uploaded media
- [ ] Form validates required fields before submission
- [ ] System displays a confirmation message with the unique tracking code after submission

---

#### Story 2: Submit Request in Low-Bandwidth or Offline Conditions

As a resident,
I want to submit a rescue request even when my internet connection is weak or unavailable,
so that I can still call for help during a disaster when signal is poor.

**Acceptance Criteria**

- [ ] System detects when the device is offline or on a low-bandwidth connection
- [ ] Rescue request form remains accessible and usable without an internet connection
- [ ] Submitted request is queued locally on the device when offline
- [ ] System displays a message informing the resident that their request will be sent when connectivity is restored
- [ ] Queued request is automatically submitted as soon as any signal is detected
- [ ] Resident receives a confirmation message with their tracking code once the queued request is successfully sent
- [ ] Map is cached and remains viewable for location selection when offline
- [ ] Previously loaded data is available offline via cache

---

## Notes (Optional)

- No user login is required to submit a request to ensure accessibility during emergencies.
- Needs are no longer selected manually by the resident. They are automatically detected from the resident's message by the triage algorithm on the backend.
- Low-bandwidth optimization is implemented on the resident side only using service workers and PWA caching strategies.
