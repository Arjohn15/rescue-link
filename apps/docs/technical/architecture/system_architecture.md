# System Architecture

## Overview

This document describes the high-level architecture of AlertoHub. The system is composed of three client-facing frontends, a NestJS backend, a PostgreSQL database, a real-time layer via Socket.IO, and two external services.

For the visual diagram, see `../design/architecture.png`.

---

## Architecture Diagram

```
Resident PWA       Admin dashboard     Rescuer portal
(Next.js + PWA)    (Next.js)           (Next.js)
      |                  |                   |
      └──────────────────┼───────────────────┘
                         |
               NestJS backend
          (REST API + WebSocket gateway)
                         |
         ┌───────────────┼───────────────┐
         |               |               |
    PostgreSQL        Socket.IO     External services
    (Prisma ORM)   (Real-time)    (Gmail + Google Maps)
         |               |               |
         └───────────────┼───────────────┘
                         |
                   AWS (planned)
```

---

## Components

### Clients

| Client          | Tech              | Description                                                                                          |
| --------------- | ----------------- | ---------------------------------------------------------------------------------------------------- |
| Resident PWA    | Next.js + Workbox | Public-facing app for submitting rescue requests. Supports offline use and low-bandwidth conditions. |
| Admin dashboard | Next.js           | Protected interface for managing rescue requests, teams, and emergency information.                  |
| Rescuer portal  | Next.js           | Separate portal for rescue team leads and members to view assignments and broadcast GPS location.    |

### Backend

| Component         | Tech      | Description                                                                                      |
| ----------------- | --------- | ------------------------------------------------------------------------------------------------ |
| REST API          | NestJS    | Handles all HTTP requests. Enforces JWT authentication for admin and rescuer routes.             |
| WebSocket gateway | Socket.IO | Manages real-time events including chat messages, live GPS location updates, and status changes. |

### Data layer

| Component | Tech                | Description                                                                                        |
| --------- | ------------------- | -------------------------------------------------------------------------------------------------- |
| Database  | PostgreSQL + Prisma | Stores all application data including rescue requests, teams, messages, and emergency information. |

### External services

| Service         | Purpose                                                                         |
| --------------- | ------------------------------------------------------------------------------- |
| Gmail API       | Sends invite emails to rescuers for account registration.                       |
| Google Maps API | Provides map rendering, location selection, and route drawing for GPS tracking. |

### Deployment

| Target | Description                                                  |
| ------ | ------------------------------------------------------------ |
| AWS    | Planned deployment target for both the frontend and backend. |

---

## Key Design Decisions

- **Separate client interfaces** — Residents, admins, and rescuers each have a dedicated interface optimized for their context and device.
- **Separate authentication** — Admins and rescuers authenticate through separate endpoints and receive distinct JWT tokens, preventing cross-role access.
- **No login for residents** — Rescue request submission and tracking are publicly accessible to ensure accessibility during emergencies.
- **Real-time via Socket.IO** — Chat, live GPS location, and status updates are handled through WebSockets to avoid polling and reduce latency.
- **PWA for residents only** — Offline support and request queuing are implemented on the resident side only, addressing real-world low-connectivity conditions during disasters.
- **Invite-based rescuer registration** — Admin accounts are seeded directly. Rescuer accounts are created via email invite, ensuring no unauthorized self-registration.

---

## Notes

- The admin dashboard and rescuer portal require stable internet connectivity.
- Real-time features such as live GPS tracking stop automatically when a request is marked as completed.
- Current operational scope is limited to the Bicutan area within Taguig City: Barangay Lower Bicutan, Upper Bicutan, Central Bicutan, and New Lower Bicutan.
- Future improvement: extend offline support to the rescuer portal for field use on low-signal deployments.
