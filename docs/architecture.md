# Subscription Product Support Desk

## 1. Project Overview

Subscription Product Support Desk is a SaaS customer-support platform that combines traditional ticket management with Retrieval-Augmented Generation (RAG) and an AI support agent.

Customers can search product documentation, ask support questions, investigate account-specific problems, and escalate unresolved issues to human support specialists.

The system uses a fictional SaaS product with seeded organizations, accounts, subscription plans, help articles, and service incidents.

---

## 2. Goals

The system should:

- Provide a searchable help center.
- Answer customer questions using trusted product documentation.
- Provide source-backed AI responses.
- Investigate authorized account-specific questions.
- Check subscription entitlements.
- Check simulated service incidents.
- Maintain customer support tickets.
- Allow customers to escalate issues.
- Provide human specialists with the conversation and investigation evidence.
- Allow knowledge managers to publish versioned help articles.
- Allow supervisors to assign tickets to specialists.
- Maintain auditability of important actions.

---

## 3. Technology Stack

### Frontend

- React
- JavaScript
- React Router
- Fetch/Axios

### Backend

- Node.js
- Express
- JavaScript
- ES Modules
- Zod
- JWT
- bcryptjs

### Database

- MongoDB Atlas
- Mongoose
- MongoDB Atlas Vector Search

### AI

- Ollama
- LangChain JavaScript
- Nemotron 3 Super
- qwen3-embedding:4b

### Development

- Git
- GitHub
- VS Code
- Postman

---

## 4. High-Level Architecture

```text
React Frontend
      |
      | HTTP REST API
      v
Node.js + Express
      |
      +-------------------+
      |                   |
      v                   v
MongoDB Atlas          AI Layer
                          |
              +-----------+-----------+
              |           |           |
             RAG        Tools       Agent
              |           |           |
              v           v           v
        Vector Search   MongoDB    Nemotron
```

---

## 5. Backend Request Flow

```text
HTTP Request
     |
     v
Express Route
     |
     v
Middleware
     |
     +--> Authentication
     |
     +--> Authorization
     |
     v
Controller
     |
     v
Validation
     |
     v
Service
     |
     v
Mongoose
     |
     v
MongoDB Atlas
     |
     v
Response
```

---

## 6. AI Request Flow

```text
Customer Question
       |
       v
Support API
       |
       v
AI Agent
       |
       +----------------------+
       |          |           |
       v          v           v
      RAG      Account     Status
       |         Tool        Tool
       v          |           |
Vector Search     +-----+-----+
       |                |
       +--------+-------+
                |
                v
             Nemotron
                |
                v
        Structured Response
                |
                v
             Customer
```

---

## 7. Security Principle

The AI model must never be responsible for authorization.

Authorization is enforced by backend application code.

```text
AI requests tool
       |
       v
Backend authorization
       |
       +---- denied ----> reject
       |
       +---- allowed ---> execute tool
```

The model cannot bypass application permissions through its prompt.

---

## 8. AI Boundaries

The AI may:

- Search documentation.
- Read authorized account information.
- Read authorized subscription information.
- Check service incidents.
- Analyze retrieved information.
- Recommend a resolution.
- Recommend escalation.

The AI may not directly:

- Issue refunds.
- Change subscriptions.
- Cancel subscriptions.
- Modify billing.
- Perform restricted account mutations.

Those actions require explicit human-controlled workflows.

---

## 9. RAG Architecture

Knowledge articles are versioned.

Published articles are transformed into searchable chunks.

```text
Help Article
     |
     v
Chunking
     |
     v
qwen3-embedding:4b
     |
     v
Embedding Vector
     |
     v
HelpChunk
     |
     v
MongoDB Atlas Vector Search
```

At query time:

```text
Customer Question
     |
     v
Embedding
     |
     v
Vector Search
     |
     v
Relevant Chunks
     |
     v
Context
     |
     v
Nemotron
     |
     v
Source-backed Answer
```

---

## 10. Human Escalation

When AI cannot safely resolve an issue:

```text
Customer
   |
   v
AI Investigation
   |
   v
Needs Human
   |
   v
Ticket Escalation
   |
   v
Supervisor Assignment
   |
   v
Support Specialist
   |
   v
Human Resolution
```

The specialist receives:

- Customer conversation
- AI responses
- Retrieved knowledge sources
- Account checks
- Entitlement checks
- Incident checks
- AI recommendation
- Escalation reason

---

## 11. Reliability Principles

The system should gracefully handle:

- MongoDB failures
- Ollama failures
- LLM failures
- Embedding failures
- Vector-search failures
- Invalid requests
- Unauthorized access
- Expired authentication
- Missing knowledge
- Invalid tickets
- Tool failures

AI failure should not bring down the entire support system.

---

## 12. Core Design Principle

The platform separates:

### Knowledge

What the product documentation says.

### Data

What is true about the customer's account.

### Investigation

What the AI determines by combining knowledge and authorized data.

### Action

What a human or explicitly authorized workflow is allowed to change.

This separation prevents the AI from becoming an uncontrolled business-action system.