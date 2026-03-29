# 🚑 Rescue Link Taguig

## 📌 Overview

**Rescue Link Taguig** is a real-time emergency response and rescue coordination system designed for disaster scenarios such as typhoons and floods. It enables authorities to locate victims, manage rescue requests, and coordinate teams efficiently within Taguig City.

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
  - name
  - message
  - contact number  
  - location  
  - needs (food, water, medical, etc.)
- No login required for accessibility  

### Real-Time Location Tracking
- Each request includes location data  
- Displayed on a map for easy identification by responders  

### Centralized Admin Dashboard
- Displays all rescue requests in one interface  
- Includes:
  - request list  
  - status  
  - priority  
  - map view  

### Request Status Management
- Admins can update request status:
  - Pending  
  - Responding  
  - In Progress  
  - Completed  

### Live Updates & Emergency Information
- Provides:
  - emergency announcements  
  - hotlines  
  - optional live broadcast  

---

## 🏗️ System Architecture

* **Frontend:** React / Next.js (App Router)
* **Backend:** Node.js (NestJS)
* **Database:** PostgreSQL
* **Deployment (Planned):** AWS

### High-Level Flow
- Users submit rescue requests
- Requests are processed and stored
- Admin dashboard receives updates in real-time
- Responders coordinate actions based on request data

---

## 🧑‍💻 Tech Stack

**Frontend**

* React / Next.js
* TypeScript
* Tailwind CSS

**Backend**

* Node.js (NestJS)
* REST API + WebSocket Gateway

**Database**

* PostgreSQL

**Real-Time**

* Socket.IO

**Cloud / DevOps**

* AWS

---

````

---

## 🚀 Running the Project (Optional)

For developers who want to explore locally:

```bash
git clone https://github.com/your-username/rescue-link-taguig.git
cd rescue-link-taguig
npm install
npm run dev
````

---

## 📸 Screenshots

_Screenshots will be added once the UI is finalized._

---

## 👥 Team

* **Project Lead / Lead Developer:** Arjohn Banado
* John Lawrence Amihan
* Mark Dennis Concha
* Bob Geof Tortogo

---

## 📚 Future Improvements

* Mobile app (React Native)
* AI-based request prioritization
* Offline-first capability
* Government system integration
* SMS fallback notifications

---

## 📬 Contact

* Email: ajbanado15@gmail.com
* GitHub: https://github.com/Arjohn15/

---

⭐ *If this project inspires you, consider giving it a star!*
