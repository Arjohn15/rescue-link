# Entity Relationship Diagram

## Overview

This document defines the database schema for RescueLink. The ERD is designed using [dbdiagram.io](https://dbdiagram.io).

For the visual diagram, see `../design/erd.png`.

---

## Tables

| Table                 | Description                                                  |
| --------------------- | ------------------------------------------------------------ |
| `admins`              | Stores admin credentials and identity                        |
| `rescue_teams`        | Stores rescue teams created and managed by admins            |
| `rescue_team_members` | Stores individual members belonging to a rescue team         |
| `rescue_requests`     | Core table storing all resident rescue requests              |
| `request_needs`       | Stores the detected needs per rescue request                 |
| `request_media`       | Stores uploaded images and videos per rescue request         |
| `messages`            | Stores chat messages between admin, team lead, and resident  |
| `alerts`              | Admin-posted emergency alerts                                |
| `hotlines`            | Emergency contact numbers managed by admins                  |
| `resources`           | Rescue resources such as evacuation centers and relief goods |
| `live_streams`        | Admin-managed live stream URLs                               |

---

## Schema

```dbml
// RescueLink - Taguig Emergency Platform
// ERD for dbdiagram.io

// ─── Enums ─────────────────────────────────────────

Enum alert_type {
  info
  advisory
  critical
}

Enum hotline_category {
  police
  fire
  medical
  disaster
  barangay
}

Enum resource_category {
  evacuation_center
  relief_goods
  medical
  rescue_team
  food_water
}

Enum request_status {
  PENDING
  IN_PROGRESS
  COMPLETED
}

Enum request_need {
  food
  water
  medical
  shelter
  rescue
  clothing
  other
}

Enum media_type {
  image
  video
}

Enum sender_role {
  admin
  team_lead
  resident
}

Enum team_member_role {
  lead
  member
}

Enum team_status {
  available
  deployed
  off_duty
}

Enum invite_status {
  pending
  accepted
  expired
}

// ─── Tables ────────────────────────────────────────

Table admins {
  id          Int      [pk, increment]
  name        String
  email       String   [unique]
  password    String
  created_at  DateTime [default: `now()`]
  updated_at  DateTime [default: `now()`]
}

Table rescue_teams {
  id          Int         [pk, increment]
  name        String
  status      team_status [default: 'available']
  created_by  Int         [ref: > admins.id]
  created_at  DateTime    [default: `now()`]
  updated_at  DateTime    [default: `now()`]
}

Table rescue_team_members {
  id              Int              [pk, increment]
  team_id         Int              [ref: > rescue_teams.id]
  name            String           [null]
  email           String           [unique]
  password        String           [null]
  role            team_member_role
  invite_token    String           [null, unique]
  invite_status   invite_status    [default: 'pending']
  invite_expires_at DateTime       [null]
  invited_by      Int              [ref: > admins.id]
  created_at      DateTime         [default: `now()`]
  updated_at      DateTime         [default: `now()`]
}

Table rescue_requests {
  id               Int            [pk, increment]
  tracking_code    String         [unique]
  full_name        String
  contact_number   String
  latitude         Float
  longitude        Float
  location_address String
  message          String
  triage_score     Float          [null]
  suggested_needs  String[]       [null]
  status           request_status [default: 'PENDING']
  claimed_by       Int            [null, ref: > admins.id]
  assigned_team    Int            [null, ref: > rescue_teams.id]
  created_at       DateTime       [default: `now()`]
  updated_at       DateTime       [default: `now()`]
}

Table request_needs {
  id          Int          [pk, increment]
  request_id  Int          [ref: > rescue_requests.id]
  need        request_need
}

Table request_media {
  id          Int        [pk, increment]
  request_id  Int        [ref: > rescue_requests.id]
  file_url    String
  file_type   media_type
  file_size   Int
  created_at  DateTime   [default: `now()`]
}

Table messages {
  id          Int         [pk, increment]
  request_id  Int         [ref: > rescue_requests.id]
  sender_role sender_role
  sender_id   Int
  content     String
  created_at  DateTime    [default: `now()`]
}

Table alerts {
  id          Int        [pk, increment]
  admin_id    Int        [ref: > admins.id]
  title       String
  body        String
  type        alert_type
  is_active   Boolean    [default: true]
  created_at  DateTime   [default: `now()`]
  updated_at  DateTime   [default: `now()`]
}

Table hotlines {
  id          Int              [pk, increment]
  admin_id    Int              [ref: > admins.id]
  name        String
  number      String
  category    hotline_category
  created_at  DateTime         [default: `now()`]
  updated_at  DateTime         [default: `now()`]
}

Table resources {
  id          Int               [pk, increment]
  admin_id    Int               [ref: > admins.id]
  title       String
  description String
  category    resource_category
  location    String            [null]
  created_at  DateTime          [default: `now()`]
  updated_at  DateTime          [default: `now()`]
}

Table live_streams {
  id          Int      [pk, increment]
  admin_id    Int      [ref: > admins.id]
  stream_url  String
  is_active   Boolean  [default: true]
  created_at  DateTime [default: `now()`]
  updated_at  DateTime [default: `now()`]
}
```
