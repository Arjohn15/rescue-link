# Rescue Teams

## Description

This feature allows administrators to create and manage rescue teams within the system. Each team consists of one lead and one or more members. Admins invite rescuers via email using the Gmail API, and rescuers set their own password upon accepting the invite. The team lead's device is used for real-time GPS tracking during deployment.

---

## User Roles Involved

- Admin

---

## User Stories

### Admin

#### Story 1: Create a Rescue Team

As an admin,
I want to create a rescue team,
so that I can organize responders into structured teams ready for deployment.

**Acceptance Criteria**

- [ ] Admin can create a new rescue team with a team name
- [ ] Newly created teams have a default status of available
- [ ] Admin can edit an existing team name
- [ ] Admin can delete a rescue team
- [ ] System displays an appropriate empty state message if no teams have been created yet

---

#### Story 2: Invite Rescuers to a Team

As an admin,
I want to invite rescuers to a team via email,
so that they can set up their own credentials and access the rescuer portal.

**Acceptance Criteria**

- [ ] Admin can invite a rescuer by inputting their email, selecting their team, and assigning their role (lead or member)
- [ ] System sends an invite email to the rescuer with a unique registration link via Gmail API
- [ ] Invite link directs the rescuer to a password setup form
- [ ] Rescuer sets their own password to activate their account
- [ ] Account is activated only after the rescuer completes the password setup
- [ ] Invite link expires after 24 hours if not used
- [ ] Admin can resend an expired or pending invite
- [ ] Admin can cancel a pending invite
- [ ] Only one lead can be assigned per team
- [ ] Each rescuer belongs to one team only
- [ ] System displays an error if admin tries to assign a second lead to a team that already has one

---

#### Story 3: Manage Team Members

As an admin,
I want to manage the members of each rescue team,
so that I can keep team compositions accurate and up to date.

**Acceptance Criteria**

- [ ] Admin can view a list of all members in a team
- [ ] Each member entry shows their name, email, role, and invite status (pending, accepted, expired)
- [ ] Admin can edit a member's role within the team
- [ ] Admin can remove a member from the team
- [ ] Removing the team lead prompts the admin to assign a new lead or leave the position vacant
- [ ] Admin can view all pending invites and their expiry status

---

#### Story 4: Monitor Rescue Team Status

As an admin,
I want to view and monitor the status of all rescue teams,
so that I can know which teams are available for deployment.

**Acceptance Criteria**

- [ ] Admin can view a list of all rescue teams
- [ ] Each team shows its name, current status, and assigned request if any
- [ ] Team status is one of: available, deployed, off duty
- [ ] Team status updates automatically based on deployment activity
- [ ] Admin can manually set a team status to off duty
- [ ] Admin can manually set a team status back to available from off duty
- [ ] Teams marked as off duty are not shown as options when assigning to a request

---

## Notes (Optional)

- Rescue team accounts are created and managed solely by admins. No self-registration is available.
- A rescuer belongs to one team only and cannot be assigned to multiple teams simultaneously.
- Only one lead is allowed per team. The lead's device is used for real-time GPS tracking during deployment.
- Team status changes to deployed automatically when the team is assigned to an active request and changes back to available when the request is marked as completed.
