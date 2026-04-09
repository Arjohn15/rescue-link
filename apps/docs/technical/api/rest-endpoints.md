# API Design

## Overview

This document defines the REST API endpoints for RescueLink. All endpoints are prefixed with `/api`. The API is built with NestJS and uses JWT for authentication on protected routes.

---

## Authentication

RescueLink has two separate authentication flows:

| Actor                    | Endpoint                       | Token Type |
| ------------------------ | ------------------------------ | ---------- |
| Admin                    | `POST /api/auth/admin/login`   | JWT        |
| Rescuer (lead or member) | `POST /api/auth/rescuer/login` | JWT        |

Protected routes require a valid JWT token passed in the request header:

```
Authorization: Bearer <token>
```

Public routes (rescue request submission, tracking, emergency information) do not require authentication.

---

## Response Format

### Success

```json
{
  "data": {},
  "message": "Success"
}
```

### Error

```json
{
  "statusCode": 404,
  "message": "Request not found",
  "error": "Not Found"
}
```

---

## HTTP Status Codes

| Code | Meaning               |
| ---- | --------------------- |
| 200  | OK                    |
| 201  | Created               |
| 400  | Bad Request           |
| 401  | Unauthorized          |
| 403  | Forbidden             |
| 404  | Not Found             |
| 500  | Internal Server Error |

---

## Endpoints

---

### Auth

#### Admin Login

```
POST /api/auth/admin/login
```

Request:

```json
{
  "email": "admin@rescuelink.com",
  "password": "password"
}
```

Response:

```json
{
  "data": {
    "access_token": "eyJhbGci...",
    "admin": {
      "id": 1,
      "name": "Admin",
      "email": "admin@rescuelink.com"
    }
  },
  "message": "Login successful"
}
```

---

#### Rescuer Login

```
POST /api/auth/rescuer/login
```

Request:

```json
{
  "email": "lead@rescuelink.com",
  "password": "password"
}
```

Response:

```json
{
  "data": {
    "access_token": "eyJhbGci...",
    "member": {
      "id": 1,
      "name": "Juan dela Cruz",
      "email": "lead@rescuelink.com",
      "role": "lead",
      "team_id": 1
    }
  },
  "message": "Login successful"
}
```

---

#### Accept Invite and Set Password

```
POST /api/auth/rescuer/accept-invite
```

Request:

```json
{
  "token": "abc123xyz",
  "name": "Juan dela Cruz",
  "password": "newpassword"
}
```

Response:

```json
{
  "data": null,
  "message": "Account activated successfully"
}
```

---

### Rescue Requests

#### Submit Rescue Request (Public)

```
POST /api/rescue-requests
```

Request (multipart/form-data):

```json
{
  "full_name": "Maria Santos",
  "contact_number": "09171234567",
  "latitude": 14.5217,
  "longitude": 121.0507,
  "location_address": "Barangay Ususan, Taguig City",
  "message": "We are trapped on the second floor, my grandfather is injured"
}
```

Response:

```json
{
  "data": {
    "id": 1,
    "tracking_code": "RES-2024-ABCD",
    "status": "PENDING",
    "triage_score": 5,
    "suggested_needs": ["rescue", "medical"]
  },
  "message": "Rescue request submitted successfully"
}
```

---

#### Get All Rescue Requests (Admin)

```
GET /api/rescue-requests
Authorization: Bearer <token>
```

Response:

```json
{
  "data": [
    {
      "id": 1,
      "tracking_code": "RES-2024-ABCD",
      "full_name": "Maria Santos",
      "status": "PENDING",
      "triage_score": 5,
      "suggested_needs": ["rescue", "medical"],
      "created_at": "2024-01-01T00:00:00.000Z"
    }
  ],
  "message": "Success"
}
```

---

#### Get Single Rescue Request (Admin)

```
GET /api/rescue-requests/:id
Authorization: Bearer <token>
```

Response:

```json
{
  "data": {
    "id": 1,
    "tracking_code": "RES-2024-ABCD",
    "full_name": "Maria Santos",
    "contact_number": "09171234567",
    "latitude": 14.5217,
    "longitude": 121.0507,
    "location_address": "Barangay Ususan, Taguig City",
    "message": "We are trapped on the second floor, my grandfather is injured",
    "status": "PENDING",
    "triage_score": 5,
    "suggested_needs": ["rescue", "medical"],
    "needs": [],
    "media": [],
    "claimed_by": null,
    "assigned_team": null,
    "created_at": "2024-01-01T00:00:00.000Z"
  },
  "message": "Success"
}
```

---

#### Track Rescue Request by Tracking Code (Public)

```
GET /api/rescue-requests/track/:tracking_code
```

Response:

```json
{
  "data": {
    "tracking_code": "RES-2024-ABCD",
    "status": "IN_PROGRESS",
    "triage_score": 5,
    "needs": ["rescue", "medical"],
    "assigned_team": {
      "id": 1,
      "name": "Team Alpha"
    },
    "created_at": "2024-01-01T00:00:00.000Z"
  },
  "message": "Success"
}
```

---

#### Claim Rescue Request (Admin)

```
PATCH /api/rescue-requests/:id/claim
Authorization: Bearer <token>
```

Response:

```json
{
  "data": {
    "id": 1,
    "claimed_by": 1,
    "status": "PENDING"
  },
  "message": "Request claimed successfully"
}
```

---

#### Assign Rescue Team (Admin)

```
PATCH /api/rescue-requests/:id/assign-team
Authorization: Bearer <token>
```

Request:

```json
{
  "team_id": 1
}
```

Response:

```json
{
  "data": {
    "id": 1,
    "assigned_team": 1,
    "status": "IN_PROGRESS"
  },
  "message": "Rescue team assigned successfully"
}
```

---

#### Update Request Status (Admin or Team Lead)

```
PATCH /api/rescue-requests/:id/status
Authorization: Bearer <token>
```

Request:

```json
{
  "status": "COMPLETED"
}
```

Response:

```json
{
  "data": {
    "id": 1,
    "status": "COMPLETED"
  },
  "message": "Status updated successfully"
}
```

---

#### Override Suggested Needs (Admin)

```
PATCH /api/rescue-requests/:id/needs
Authorization: Bearer <token>
```

Request:

```json
{
  "needs": ["medical", "food", "shelter"]
}
```

Response:

```json
{
  "data": {
    "id": 1,
    "needs": ["medical", "food", "shelter"]
  },
  "message": "Needs updated successfully"
}
```

---

### Messages

#### Get Messages for a Request (Admin or Team Lead)

```
GET /api/rescue-requests/:id/messages
Authorization: Bearer <token>
```

Response:

```json
{
  "data": [
    {
      "id": 1,
      "sender_role": "admin",
      "sender_id": 1,
      "content": "Help is on the way",
      "created_at": "2024-01-01T00:00:00.000Z"
    },
    {
      "id": 2,
      "sender_role": "resident",
      "sender_id": null,
      "content": "Please hurry",
      "created_at": "2024-01-01T00:01:00.000Z"
    }
  ],
  "message": "Success"
}
```

---

#### Get Messages by Tracking Code (Resident)

```
GET /api/rescue-requests/track/:tracking_code/messages
```

Response:

```json
{
  "data": [
    {
      "id": 1,
      "sender_role": "admin",
      "content": "Help is on the way",
      "created_at": "2024-01-01T00:00:00.000Z"
    }
  ],
  "message": "Success"
}
```

---

#### Send Message (Admin or Team Lead)

```
POST /api/rescue-requests/:id/messages
Authorization: Bearer <token>
```

Request:

```json
{
  "content": "We are 5 minutes away"
}
```

Response:

```json
{
  "data": {
    "id": 3,
    "sender_role": "team_lead",
    "sender_id": 1,
    "content": "We are 5 minutes away",
    "created_at": "2024-01-01T00:02:00.000Z"
  },
  "message": "Message sent successfully"
}
```

---

#### Send Message by Tracking Code (Resident)

```
POST /api/rescue-requests/track/:tracking_code/messages
```

Request:

```json
{
  "content": "We are on the rooftop"
}
```

Response:

```json
{
  "data": {
    "id": 4,
    "sender_role": "resident",
    "content": "We are on the rooftop",
    "created_at": "2024-01-01T00:03:00.000Z"
  },
  "message": "Message sent successfully"
}
```

---

### Rescue Teams

#### Get All Rescue Teams (Admin)

```
GET /api/rescue-teams
Authorization: Bearer <token>
```

Response:

```json
{
  "data": [
    {
      "id": 1,
      "name": "Team Alpha",
      "status": "available",
      "members": [
        {
          "id": 1,
          "name": "Juan dela Cruz",
          "role": "lead",
          "invite_status": "accepted"
        }
      ]
    }
  ],
  "message": "Success"
}
```

---

#### Create Rescue Team (Admin)

```
POST /api/rescue-teams
Authorization: Bearer <token>
```

Request:

```json
{
  "name": "Team Bravo"
}
```

Response:

```json
{
  "data": {
    "id": 2,
    "name": "Team Bravo",
    "status": "available"
  },
  "message": "Rescue team created successfully"
}
```

---

#### Update Rescue Team (Admin)

```
PATCH /api/rescue-teams/:id
Authorization: Bearer <token>
```

Request:

```json
{
  "name": "Team Bravo Updated",
  "status": "off_duty"
}
```

Response:

```json
{
  "data": {
    "id": 2,
    "name": "Team Bravo Updated",
    "status": "off_duty"
  },
  "message": "Rescue team updated successfully"
}
```

---

#### Delete Rescue Team (Admin)

```
DELETE /api/rescue-teams/:id
Authorization: Bearer <token>
```

Response:

```json
{
  "data": null,
  "message": "Rescue team deleted successfully"
}
```

---

#### Invite Rescuer to Team (Admin)

```
POST /api/rescue-teams/:id/invite
Authorization: Bearer <token>
```

Request:

```json
{
  "email": "member@rescuelink.com",
  "role": "member"
}
```

Response:

```json
{
  "data": {
    "id": 2,
    "email": "member@rescuelink.com",
    "role": "member",
    "invite_status": "pending"
  },
  "message": "Invite sent successfully"
}
```

---

#### Resend Invite (Admin)

```
POST /api/rescue-teams/members/:member_id/resend-invite
Authorization: Bearer <token>
```

Response:

```json
{
  "data": null,
  "message": "Invite resent successfully"
}
```

---

#### Remove Team Member (Admin)

```
DELETE /api/rescue-teams/members/:member_id
Authorization: Bearer <token>
```

Response:

```json
{
  "data": null,
  "message": "Member removed successfully"
}
```

---

### Alerts

#### Get All Active Alerts (Public)

```
GET /api/alerts
```

Response:

```json
{
  "data": [
    {
      "id": 1,
      "title": "Typhoon Warning",
      "body": "Typhoon signal number 2 is raised over Taguig City",
      "type": "critical",
      "is_active": true,
      "created_at": "2024-01-01T00:00:00.000Z"
    }
  ],
  "message": "Success"
}
```

---

#### Create Alert (Admin)

```
POST /api/alerts
Authorization: Bearer <token>
```

Request:

```json
{
  "title": "Typhoon Warning",
  "body": "Typhoon signal number 2 is raised over Taguig City",
  "type": "critical",
  "is_active": true
}
```

Response:

```json
{
  "data": {
    "id": 1,
    "title": "Typhoon Warning",
    "type": "critical",
    "is_active": true,
    "created_at": "2024-01-01T00:00:00.000Z"
  },
  "message": "Alert created successfully"
}
```

---

#### Update Alert (Admin)

```
PATCH /api/alerts/:id
Authorization: Bearer <token>
```

Request:

```json
{
  "is_active": false
}
```

Response:

```json
{
  "data": {
    "id": 1,
    "is_active": false
  },
  "message": "Alert updated successfully"
}
```

---

#### Delete Alert (Admin)

```
DELETE /api/alerts/:id
Authorization: Bearer <token>
```

Response:

```json
{
  "data": null,
  "message": "Alert deleted successfully"
}
```

---

### Hotlines

#### Get All Hotlines (Public)

```
GET /api/hotlines
```

Response:

```json
{
  "data": [
    {
      "id": 1,
      "name": "Taguig City Police",
      "number": "02-8838-0000",
      "category": "police"
    }
  ],
  "message": "Success"
}
```

---

#### Create Hotline (Admin)

```
POST /api/hotlines
Authorization: Bearer <token>
```

Request:

```json
{
  "name": "Taguig City Police",
  "number": "02-8838-0000",
  "category": "police"
}
```

Response:

```json
{
  "data": {
    "id": 1,
    "name": "Taguig City Police",
    "number": "02-8838-0000",
    "category": "police"
  },
  "message": "Hotline created successfully"
}
```

---

#### Update Hotline (Admin)

```
PATCH /api/hotlines/:id
Authorization: Bearer <token>
```

Request:

```json
{
  "number": "02-8838-9999"
}
```

Response:

```json
{
  "data": {
    "id": 1,
    "number": "02-8838-9999"
  },
  "message": "Hotline updated successfully"
}
```

---

#### Delete Hotline (Admin)

```
DELETE /api/hotlines/:id
Authorization: Bearer <token>
```

Response:

```json
{
  "data": null,
  "message": "Hotline deleted successfully"
}
```

---

### Resources

#### Get All Resources (Public)

```
GET /api/resources
```

Response:

```json
{
  "data": [
    {
      "id": 1,
      "title": "Ususan Evacuation Center",
      "description": "Located at Barangay Ususan covered court",
      "category": "evacuation_center",
      "location": "Barangay Ususan, Taguig City"
    }
  ],
  "message": "Success"
}
```

---

#### Create Resource (Admin)

```
POST /api/resources
Authorization: Bearer <token>
```

Request:

```json
{
  "title": "Ususan Evacuation Center",
  "description": "Located at Barangay Ususan covered court",
  "category": "evacuation_center",
  "location": "Barangay Ususan, Taguig City"
}
```

Response:

```json
{
  "data": {
    "id": 1,
    "title": "Ususan Evacuation Center",
    "category": "evacuation_center"
  },
  "message": "Resource created successfully"
}
```

---

#### Update Resource (Admin)

```
PATCH /api/resources/:id
Authorization: Bearer <token>
```

Request:

```json
{
  "description": "Now located at the main gymnasium"
}
```

Response:

```json
{
  "data": {
    "id": 1,
    "description": "Now located at the main gymnasium"
  },
  "message": "Resource updated successfully"
}
```

---

#### Delete Resource (Admin)

```
DELETE /api/resources/:id
Authorization: Bearer <token>
```

Response:

```json
{
  "data": null,
  "message": "Resource deleted successfully"
}
```

---

### Live Streams

#### Get Active Live Stream (Public)

```
GET /api/live-streams/active
```

Response:

```json
{
  "data": {
    "id": 1,
    "stream_url": "https://www.youtube.com/watch?v=example",
    "is_active": true
  },
  "message": "Success"
}
```

---

#### Create Live Stream (Admin)

```
POST /api/live-streams
Authorization: Bearer <token>
```

Request:

```json
{
  "stream_url": "https://www.youtube.com/watch?v=example"
}
```

Response:

```json
{
  "data": {
    "id": 1,
    "stream_url": "https://www.youtube.com/watch?v=example",
    "is_active": true
  },
  "message": "Live stream created successfully"
}
```

---

#### Deactivate Live Stream (Admin)

```
PATCH /api/live-streams/:id
Authorization: Bearer <token>
```

Request:

```json
{
  "is_active": false
}
```

Response:

```json
{
  "data": {
    "id": 1,
    "is_active": false
  },
  "message": "Live stream deactivated successfully"
}
```

---

#### Delete Live Stream (Admin)

```
DELETE /api/live-streams/:id
Authorization: Bearer <token>
```

Response:

```json
{
  "data": null,
  "message": "Live stream deleted successfully"
}
```

---

## Notes

- Public endpoints do not require authentication and are accessible to all users.
- Admin endpoints require a valid JWT token issued from `POST /api/auth/admin/login`.
- Rescuer endpoints require a valid JWT token issued from `POST /api/auth/rescuer/login`.
- Real-time features such as chat, live location, and status updates are handled via Socket.IO and are documented separately in `websocket-events.md`.
- File uploads for rescue request media use `multipart/form-data` content type.
