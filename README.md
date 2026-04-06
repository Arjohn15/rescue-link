# RescueLink - Taguig Emergency Platform

## Overview

**RescueLink** is a real-time emergency response and rescue coordination system designed for disaster scenarios such as typhoons and floods. It enables authorities to locate victims, manage rescue requests, and coordinate rescue teams efficiently within Taguig City.

This project is a thesis system demonstrating real-time coordination, automated triage, and modern web development practices.

---

## Problem Definition

During large-scale disasters such as typhoons and flooding, emergency response coordination becomes increasingly challenging. Rescue requests submitted through calls, social media, or messaging platforms are difficult to consolidate, track, and prioritize in real time. This gap can lead to delayed responses and inefficient use of rescue resources, a challenge that many local government units, including those in Taguig City, continue to work toward addressing.

---

## Target Users

### Primary Users

- Residents of Taguig City
  - Individuals affected by disasters who need rescue or assistance

### Secondary Users

- Rescue Coordinators / Emergency Responders
  - Personnel responsible for monitoring, managing, and responding to rescue requests

---

## Core Features

| Feature                        | Description                                                                               |
| ------------------------------ | ----------------------------------------------------------------------------------------- |
| **Rescue Request Submission**  | Residents submit requests with location and a message. No login required.                 |
| **Automated Triage**           | Lexicon-based algorithm analyzes the message to detect needs and assign a priority score. |
| **Low-Bandwidth Optimization** | PWA with offline support and request queuing for 2G/Edge environments.                    |
| **Request Tracking**           | Residents track status and live rescuer location using a unique tracking code.            |
| **Admin Dashboard**            | Centralized command view with real-time metrics, map, and request list.                   |
| **Request Management**         | Admins claim, triage, assign teams, and update requests.                                  |
| **Rescue Team Management**     | Admins create teams and invite rescuers via email.                                        |
| **Rescuer Portal**             | Team leads broadcast GPS location during deployment. Members view assignments.            |
| **Emergency Information**      | Admins post alerts, hotlines, resources, and live streams publicly.                       |

For full feature details, see [`apps/docs/features/`](apps/docs/features/).

---

## System Architecture

- **Frontend:** React / Next.js (App Router, PWA)
- **Backend:** Node.js (NestJS)
- **Database:** PostgreSQL
- **Real-Time:** Socket.IO via WebSocket Gateway
- **Email:** Gmail API (invite-based rescuer registration)
- **Maps:** Google Maps API / Mapbox
- **Deployment (Planned):** AWS

### High-Level Flow

1. Residents submit rescue requests without requiring login
2. Backend analyzes the message using the triage algorithm and assigns a score and suggested needs
3. System stores the request and issues a unique tracking code to the resident
4. If the resident is offline, the request is queued and sent automatically when signal is restored
5. Admin dashboard receives new requests in real time with triage scores and suggested needs
6. Admins log in, claim requests, override needs if necessary, and assign rescue teams
7. Rescue team lead logs in to the rescuer portal and sees their assignment
8. Team lead's GPS location is broadcast in real time during deployment
9. Resident tracks the request status and sees the team lead moving on the map
10. Resident and admin communicate via real-time chat
11. Team lead marks the request as completed, GPS broadcasting stops, team status returns to available
12. Summary metrics and map view update in real time as statuses change

---

## Tech Stack

**Frontend**

- React / Next.js
- TypeScript
- Tailwind CSS
- PWA (next-pwa, Workbox)

**Backend**

- Node.js (NestJS)
- REST API + WebSocket Gateway
- Lexicon-Based Weighted Scoring Algorithm (Automated Triage)

**Database**

- PostgreSQL
- Prisma ORM

**Real-Time**

- Socket.IO

**Maps**

- Google Maps API / Mapbox

**Email**

- Gmail API

**Cloud / DevOps**

- AWS

---

## Running the Project

```bash
git clone https://github.com/Arjohn15/rescue-link-taguig.git
cd rescue-link-taguig
```

### Backend (API)

```bash
cd api
npm install
npm run start:dev
```

### Frontend (Web)

```bash
cd web
npm install
npm run dev
```

> Make sure to run the backend before the frontend. Both must be running simultaneously for the system to work correctly.

---

## Team

- **Project Lead / Lead Developer:** Arjohn Banado
- John Lawrence Amihan
- Mark Dennis Concha
- Bob Geof Tortogo

---

## Future Improvements

- Mobile app (React Native)
- NLP or ML-based triage for higher accuracy (replacing lexicon-based approach)
- Government system integration
- SMS fallback notifications
- Extend offline support to the rescuer portal
- Role-based access control (super admin, regular admin, viewer)

---

## License

This project is developed for academic purposes as a thesis requirement. All rights reserved by the development team.

---

## Contact

- Email: ajbanado15@gmail.com
- GitHub: https://github.com/Arjohn15/

---

_If this project inspires you, consider giving it a star!_
