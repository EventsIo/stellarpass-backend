# API Documentation

Base URL: `http://localhost:3001/api/v1`

All protected routes require the header:
```
Authorization: Bearer <jwt_token>
```

---

## Auth

### POST `/auth/register`
Register a new organizer account.

**Body**
```json
{
  "email": "organizer@example.com",
  "password": "securepassword",
  "name": "Event Co."
}
```

**Response** `201`
```json
{ "message": "Registration successful" }
```

---

### POST `/auth/login`
Login with email and password.

**Body**
```json
{
  "email": "organizer@example.com",
  "password": "securepassword"
}
```

**Response** `200`
```json
{
  "access_token": "<jwt>",
  "user": { "id": "uuid", "email": "...", "plan": "free" }
}
```

---

### POST `/auth/forgot-password`
Send password reset email.

**Body**
```json
{ "email": "organizer@example.com" }
```

---

### POST `/auth/reset-password`
Reset password using token from email.

**Body**
```json
{
  "token": "<reset_token>",
  "password": "newpassword"
}
```

---

## Events

### POST `/events` 🔒
Create a new event.

**Body**
```json
{
  "name": "Web3 Summit 2025",
  "description": "Annual Web3 conference",
  "date": "2025-09-15T10:00:00Z",
  "location": "Lagos, Nigeria",
  "eventType": "public",
  "ticketSupply": 200,
  "tiers": [
    { "name": "General", "price": 50, "currency": "USDC", "supply": 150 },
    { "name": "VIP", "price": 150, "currency": "USDC", "supply": 50 }
  ]
}
```

---

### GET `/events`
List all public events.

---

### GET `/events/:id`
Get a single event by ID.

---

### PATCH `/events/:id` 🔒
Update event details (organizer only, before event goes live).

---

### DELETE `/events/:id` 🔒
Cancel an event (organizer only).

---

## Tickets

### POST `/tickets/purchase` 🔒 (Attendee)
Purchase a ticket for an event.

**Body**
```json
{
  "eventId": "uuid",
  "tierId": "uuid",
  "currency": "USDC",
  "walletAddress": "G..."
}
```

---

### GET `/tickets/my` 🔒 (Attendee)
Get all tickets for the connected wallet.

---

### POST `/tickets/verify`
Verify a ticket by QR code data.

**Body**
```json
{ "ticketId": "uuid", "walletAddress": "G..." }
```

---

### POST `/tickets/:id/checkin` 🔒 (Organizer)
Mark a ticket as checked-in.

---

### DELETE `/tickets/:id` 🔒 (Organizer)
Revoke a ticket.

---

## Invites (Pro Plan Only)

### POST `/invites/generate` 🔒
Generate a batch of invite codes for an event.

**Body**
```json
{
  "eventId": "uuid",
  "quantity": 50,
  "expiresAt": "2025-09-14T23:59:59Z"
}
```

---

### POST `/invites/validate`
Validate an invite code before purchase.

**Body**
```json
{
  "code": "INV-XXXXXXXX",
  "eventId": "uuid"
}
```

---

### GET `/invites/:eventId` 🔒 (Organizer)
List all invite codes for an event with usage status.

---

## Analytics (Pro Plan Only)

### GET `/analytics/:eventId` 🔒
Get event analytics.

**Response**
```json
{
  "totalTicketsSold": 120,
  "revenue": { "XLM": 0, "USDC": 6000 },
  "checkInRate": "60%",
  "checkedIn": 72
}
```

---

### GET `/analytics/:eventId/export` 🔒
Export attendee list as CSV.
