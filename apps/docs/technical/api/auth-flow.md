# Authentication Flow

## Overview

AlertoHub has two separate authentication flows — one for admins and one for rescuers. Both use JWT (JSON Web Token) for session management. Residents do not require authentication and access public routes freely.

---

## Actors and Access

| Actor              | Auth required | Login route      | Token issuer                   |
| ------------------ | ------------- | ---------------- | ------------------------------ |
| Resident           | No            | None             | None                           |
| Admin              | Yes           | `/admin/login`   | `POST /api/auth/admin/login`   |
| Rescue team lead   | Yes           | `/rescuer/login` | `POST /api/auth/rescuer/login` |
| Rescue team member | Yes           | `/rescuer/login` | `POST /api/auth/rescuer/login` |

---

## Admin Authentication

### Flow

```
Admin navigates to /admin/login
        ↓
Submits email and password
        ↓
POST /api/auth/admin/login
        ↓
NestJS validates credentials against admins table
        ↓
Password verified using bcrypt
        ↓
JWT token issued with admin payload
        ↓
Token stored on the client (httpOnly cookie or localStorage)
        ↓
Admin redirected to /admin/dashboard
        ↓
All subsequent requests include JWT in Authorization header
        ↓
NestJS JwtAuthGuard validates token on every protected route
        ↓
Session expires after inactivity or token expiry
        ↓
Admin redirected to /admin/login
```

### JWT Payload

```json
{
  "sub": 1,
  "email": "admin@alertohub.com",
  "role": "admin",
  "iat": 1700000000,
  "exp": 1700086400
}
```

### Protected routes

All routes under `/api/admin/*` and any admin-specific endpoints require a valid admin JWT. Unauthenticated requests receive a `401 Unauthorized` response.

---

## Rescuer Authentication

### Invite and Registration Flow

```
Admin inputs rescuer email, team, and role in dashboard
        ↓
POST /api/rescue-teams/:id/invite
        ↓
System generates unique invite token (nanoid or crypto.randomUUID)
        ↓
invite_token and invite_expires_at saved to rescue_team_members table
        ↓
Gmail API sends invite email to rescuer
        ↓
Email contains link: /rescuer/accept-invite?token=abc123
        ↓
Rescuer clicks link, lands on password setup page
        ↓
Rescuer submits name and password
        ↓
POST /api/auth/rescuer/accept-invite
        ↓
System validates token is valid and not expired
        ↓
Password hashed with bcrypt and saved
        ↓
invite_status updated to accepted
        ↓
Rescuer redirected to /rescuer/login
```

### Login Flow

```
Rescuer navigates to /rescuer/login
        ↓
Submits email and password
        ↓
POST /api/auth/rescuer/login
        ↓
NestJS validates credentials against rescue_team_members table
        ↓
Password verified using bcrypt
        ↓
JWT token issued with rescuer payload
        ↓
Token stored on the client
        ↓
Rescuer redirected based on role:
  - lead   → /rescuer/dashboard (full access)
  - member → /rescuer/dashboard (read-only)
        ↓
All subsequent requests include JWT in Authorization header
        ↓
NestJS JwtAuthGuard validates token on every protected route
        ↓
Session expires after inactivity or token expiry
        ↓
Rescuer redirected to /rescuer/login
```

### JWT Payload

```json
{
  "sub": 1,
  "email": "lead@alertohub.com",
  "role": "lead",
  "team_id": 1,
  "iat": 1700000000,
  "exp": 1700086400
}
```

### Protected routes

All routes under `/api/rescue-teams/*` that require rescuer identity use the rescuer JWT. The `role` field in the payload determines whether the rescuer has full access (lead) or read-only access (member).

---

## Token Strategy

| Property  | Value                                             |
| --------- | ------------------------------------------------- |
| Algorithm | HS256                                             |
| Expiry    | 24 hours                                          |
| Storage   | httpOnly cookie (recommended) or localStorage     |
| Refresh   | Not implemented — user re-authenticates on expiry |

---

## Route Protection in NestJS

All protected routes use the `JwtAuthGuard`:

```ts
@UseGuards(JwtAuthGuard)
@Get('rescue-requests')
findAll() {
  return this.rescueRequestsService.findAll()
}
```

Public routes are marked explicitly with a `@Public()` decorator:

```ts
@Public()
@Post('rescue-requests')
create(@Body() dto: CreateRescueRequestDto) {
  return this.rescueRequestsService.create(dto)
}
```

---

## Invite Token Expiry

Invite tokens expire 24 hours after they are issued. If a rescuer tries to use an expired link:

- System returns a `400 Bad Request` response
- Page displays an error message informing the rescuer the link has expired
- Admin can resend the invite from the team management page which generates a new token and resets the expiry

---

## Security Notes

- Passwords are hashed using `bcrypt` before being stored in the database. Plain text passwords are never saved.
- Invite tokens are single-use. Once accepted, `invite_status` is set to `accepted` and the token is invalidated.
- Admin and rescuer tokens are issued from separate endpoints and contain different role identifiers, preventing cross-role access.
- Failed login attempts should be throttled to prevent brute force attacks. NestJS Throttler can be used for this.
- JWT secret must be stored in environment variables and never hardcoded.

---

## Notes

- Admin accounts are provisioned via a seed script. No public registration is available.
- Rescuer accounts are created solely through the admin invite flow.
- Residents do not have accounts and access the system through public routes and unique tracking codes only.
