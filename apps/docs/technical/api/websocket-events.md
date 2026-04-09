# WebSocket Events

## Overview

RescueLink uses Socket.IO via NestJS WebSocket Gateway for all real-time features. This document defines all events, their payloads, the parties involved, and the room strategy used to scope events to specific rescue requests.

---

## Room Strategy

Every rescue request has its own Socket.IO room. All real-time events related to a request are emitted only to that room, preventing data from leaking across requests.

```
room: request_{id}

Members:
  - Resident    → joins using their tracking code
  - Admin       → joins when they claim the request
  - Team lead   → joins when their team is assigned
```

### Joining a room

| Actor     | When                                 | How                                          |
| --------- | ------------------------------------ | -------------------------------------------- |
| Resident  | Opens tracking page                  | Emits `join_request` with tracking code      |
| Admin     | Opens request details                | Emits `join_request` with request id and JWT |
| Team lead | Logs in and has an active assignment | Emits `join_request` with request id and JWT |

---

## Events

---

### Connection

#### `join_request`

Emitted by any actor to join a request room.

Direction: `client → server`

Payload:

```json
{
  "tracking_code": "RES-2024-ABCD"
}
```

or for authenticated actors:

```json
{
  "request_id": 1
}
```

---

#### `leave_request`

Emitted when an actor leaves the request details view.

Direction: `client → server`

Payload:

```json
{
  "request_id": 1
}
```

---

### Rescue Requests

#### `new_request`

Emitted to all connected admins when a new rescue request is submitted.

Direction: `server → admin dashboard (broadcast)`

Payload:

```json
{
  "id": 1,
  "tracking_code": "RES-2024-ABCD",
  "full_name": "Maria Santos",
  "location_address": "Barangay Ususan, Taguig City",
  "latitude": 14.5217,
  "longitude": 121.0507,
  "triage_score": 5,
  "suggested_needs": ["rescue", "medical"],
  "status": "PENDING",
  "created_at": "2024-01-01T00:00:00.000Z"
}
```

---

#### `request_claimed`

Emitted to all connected admins when a request is claimed. Used to remove it from the unclaimed list on other admin screens.

Direction: `server → admin dashboard (broadcast)`

Payload:

```json
{
  "request_id": 1,
  "claimed_by": {
    "id": 1,
    "name": "Admin Juan"
  }
}
```

---

#### `request_status_updated`

Emitted to the request room when the status of a request changes.

Direction: `server → room request_{id}`

Payload:

```json
{
  "request_id": 1,
  "status": "IN_PROGRESS"
}
```

---

#### `metrics_updated`

Emitted to all connected admins when any request status changes, triggering a refresh of the dashboard summary cards.

Direction: `server → admin dashboard (broadcast)`

Payload:

```json
{
  "total": 24,
  "pending": 10,
  "in_progress": 8,
  "completed": 6
}
```

---

### Chat

#### `send_message`

Emitted by any actor to send a message in the request thread.

Direction: `client → server`

Payload:

```json
{
  "request_id": 1,
  "content": "We are on the rooftop"
}
```

---

#### `new_message`

Emitted to the request room when a new message is received.

Direction: `server → room request_{id}`

Payload:

```json
{
  "id": 4,
  "request_id": 1,
  "sender_role": "resident",
  "sender_id": null,
  "content": "We are on the rooftop",
  "created_at": "2024-01-01T00:03:00.000Z"
}
```

---

### GPS Location

#### `location_update`

Emitted by the team lead's device to broadcast their current GPS coordinates during deployment.

Direction: `client (team lead) → server`

Payload:

```json
{
  "request_id": 1,
  "latitude": 14.522,
  "longitude": 121.051
}
```

---

#### `team_location_updated`

Emitted to the request room when a new location update is received from the team lead. Both the resident tracking page and admin dashboard map listen for this event.

Direction: `server → room request_{id}`

Payload:

```json
{
  "request_id": 1,
  "latitude": 14.522,
  "longitude": 121.051,
  "updated_at": "2024-01-01T00:05:00.000Z"
}
```

---

#### `location_broadcasting_stopped`

Emitted to the request room when the request is marked as completed and GPS broadcasting stops automatically.

Direction: `server → room request_{id}`

Payload:

```json
{
  "request_id": 1
}
```

---

### Team Status

#### `team_status_updated`

Emitted to all connected admins when a rescue team's status changes (available, deployed, off duty).

Direction: `server → admin dashboard (broadcast)`

Payload:

```json
{
  "team_id": 1,
  "name": "Team Alpha",
  "status": "deployed"
}
```

---

## Event Summary

| Event                           | Direction       | Scope          | Description                  |
| ------------------------------- | --------------- | -------------- | ---------------------------- |
| `join_request`                  | client → server | per actor      | Join a request room          |
| `leave_request`                 | client → server | per actor      | Leave a request room         |
| `new_request`                   | server → client | broadcast      | New request submitted        |
| `request_claimed`               | server → client | broadcast      | Request claimed by admin     |
| `request_status_updated`        | server → client | room           | Request status changed       |
| `metrics_updated`               | server → client | broadcast      | Dashboard metrics refresh    |
| `send_message`                  | client → server | per actor      | Actor sends a chat message   |
| `new_message`                   | server → client | room           | New message in thread        |
| `location_update`               | client → server | team lead only | Team lead sends GPS coords   |
| `team_location_updated`         | server → client | room           | GPS coords broadcast to room |
| `location_broadcasting_stopped` | server → client | room           | GPS stopped on completion    |
| `team_status_updated`           | server → client | broadcast      | Team status changed          |

---

## Notes

- All events scoped to a room are only received by actors currently joined to that room.
- Broadcast events are received by all connected clients on the relevant interface (admin dashboard or rescuer portal).
- The resident joins the room using their tracking code only — no login required.
- GPS location broadcasting starts automatically when a team is deployed and stops automatically when the request is marked as completed.
- Future improvement: add typing indicators to the chat interface.
