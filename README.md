# 🚑 RescueLink - Taguig Emergency Platform

## 📌 Overview

**RescueLink** is a real-time emergency response and rescue coordination system designed for disaster scenarios such as typhoons and floods. It enables authorities to locate victims, manage rescue requests, and coordinate teams efficiently within Taguig City.

This project is a thesis system demonstrating real-time coordination and modern web development practices.

---

## ⁉️ Problem Definition

The current emergency response process in Taguig City is often fragmented and inefficient, especially during disasters such as typhoons and flooding. Rescue requests are commonly sent through calls, social media, or messaging platforms, making them difficult to organize, track, and prioritize. This results in delayed responses, lack of coordination, and incomplete information for responders.

---

## 🎯 Target Users

### Primary Users

- Residents of Taguig City
  - Individuals affected by disasters who need rescue or assistance

### Secondary Users

- Rescue Coordinators / Emergency Responders
  - Personnel responsible for monitoring, managing, and responding to rescue requests

---

## ⭐ Core MVP Features

### Rescue Request Submission

- Users can submit rescue requests including:
  - Name
  - Contact number
  - Location (manual input or map selection)
  - Needs (food, water, medical, etc.)
  - Optional message
  - Optional image or video attachments (with file type and size restrictions)
- No login required for accessibility
- System issues a unique tracking code upon successful submission

### Request Tracking

- Residents can track their submitted request using their unique tracking code
- Tracking page displays current status (Pending, In Progress, Completed)
- Real-time status updates without page refresh
- Residents can communicate with their assigned responder via chat from the tracking page

### Centralized Admin Dashboard

- Displays all rescue requests in one interface
- Includes:
  - Summary metrics (total, pending, in-progress, completed) with real-time updates
  - Request list with status badges and urgency indicators
  - Map view with plotted request locations
- Admins must log in with credentials to access the dashboard
- Admins can log out and sessions expire after inactivity

### Request Management

- Admins can claim unassigned requests and manage their own assigned requests
- Each admin has a dedicated "My Requests" page to avoid overlap with other admins
- Admins can update request status:
  - Pending
  - In Progress
  - Completed
- Admins can view full request details including location, needs with icons, and submitted media
- Admins can communicate with residents via real-time chat from the request details view

### Live Updates & Emergency Information

- Admins can display or post:
  - Emergency announcements
  - Hotlines
  - Optional live broadcast
  - Rescue emergency resources

---

## 🏗️ System Architecture

- **Frontend:** React / Next.js (App Router)
- **Backend:** Node.js (NestJS)
- **Database:** PostgreSQL
- **Real-Time:** Socket.IO via WebSocket Gateway
- **Deployment (Planned):** AWS

### High-Level Flow

1. Residents submit rescue requests without requiring login
2. System stores the request and issues a unique tracking code to the resident
3. Resident can track request status and chat with their assigned responder using the tracking code
4. Admin dashboard receives new requests in real-time
5. Admins log in, claim requests, and manage them from their personal request page
6. Responders update request statuses and communicate with residents via chat
7. Summary metrics and map view update in real-time as statuses change

---

## 🧑‍💻 Tech Stack

**Frontend**

- React / Next.js
- TypeScript
- Tailwind CSS

**Backend**

- Node.js (NestJS)
- REST API + WebSocket Gateway

**Database**

- PostgreSQL

**Real-Time**

- Socket.IO

**Cloud / DevOps**

- AWS

---

## 🚀 Running the Project

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

## 👥 Team

- **Project Lead / Lead Developer:** Arjohn Banado
- John Lawrence Amihan
- Mark Dennis Concha
- Bob Geof Tortogo

---

## 📚 Future Improvements

- Mobile app (React Native)
- AI-based request prioritization
- Government system integration
- SMS fallback notifications

---

## 📄 License

This project is developed for academic purposes as a thesis requirement. All rights reserved by the development team.

---

## 📬 Contact

- Email: ajbanado15@gmail.com
- GitHub: https://github.com/Arjohn15/

---

⭐ _If this project inspires you, consider giving it a star!_
