# API Specification

## Public APIs

### GET /api/help

Returns published help articles.

### GET /api/status

Returns current service incidents/status.

---

# Authentication

### POST /api/auth/register

Creates a customer account.

### POST /api/auth/login

Authenticates a user and returns a JWT.

---

# Customer APIs

### POST /api/tickets

Creates a support ticket.

### GET /api/tickets/:id

Returns a customer's authorized ticket.

### POST /api/support-chat

Ask the RAG-based support system a question.

### POST /api/support-agent

Run an AI investigation using authorized tools.

---

# Specialist APIs

### PATCH /api/tickets/:id

Updates ticket status/details.

### POST /api/tickets/:id/resolution

Adds a human resolution.

### POST /api/tickets/:id/escalation

Creates or updates escalation information.

---

# Knowledge Manager APIs

### POST /api/help-drafts

Creates a help article draft.

### POST /api/help-drafts/:id/publish

Publishes a help article version.

---

# Supervisor APIs

### POST /api/tickets/:id/assignment

Assigns a ticket to a support specialist.

---

# Authentication

Protected endpoints use:

```http
Authorization: Bearer <JWT>
```

---

# API Architecture

```text
Route
  |
  v
Authentication
  |
  v
Authorization
  |
  v
Validation
  |
  v
Controller
  |
  v
Service
  |
  v
Database / AI Tool
```

---

# Standard Success Response

```json
{
  "success": true,
  "data": {}
}
```

# Standard Error Response

```json
{
  "success": false,
  "message": "Something went wrong"
}
```