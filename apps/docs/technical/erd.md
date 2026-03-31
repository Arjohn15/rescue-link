# Entity Relationship Diagram

## Overview

This document defines the database schema for RescueLink. The ERD is designed using [dbdiagram.io](https://dbdiagram.io).

For the visual diagram, see `../design/erd.png`.

---

## Tables

| Table             | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| `admins`          | Stores admin credentials and identity                        |
| `rescue_requests` | Core table storing all resident rescue requests              |
| `request_needs`   | Stores the selected needs per rescue request                 |
| `request_media`   | Stores uploaded images and videos per rescue request         |
| `messages`        | Stores chat messages between admin and resident              |
| `alerts`          | Admin-posted emergency alerts                                |
| `hotlines`        | Emergency contact numbers managed by admins                  |
| `resources`       | Rescue resources such as evacuation centers and relief goods |
| `live_streams`    | Admin-managed live stream URLs                               |

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
   // more types may be added in the future
}

Enum hotline_category {
  police
  fire
  medical
  disaster
  barangay
   // more types may be added in the future
}

Enum resource_category {
  evacuation_center
  relief_goods
  medical
  rescue_team
  food_water
   // more types may be added in the future
}

Enum request_status {
  PENDING
  IN_PROGRESS
  COMPLETED
   // more types may be added in the future
}

Enum request_need {
  food
  water
  medical
  shelter
  rescue
  clothing
  other
   // more types may be added in the future
}

Enum media_type {
  image
  video
   // more types may be added in the future
}

Enum sender_role {
  admin
  resident
   // more types may be added in the future
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

Table rescue_requests {
  id               Int            [pk, increment]
  tracking_code    String         [unique]
  full_name        String
  contact_number   String
  latitude         Float
  longitude        Float
  location_address String
  message          String         [null]
  status           request_status [default: 'PENDING']
  claimed_by       Int            [null, ref: > admins.id]
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
