# AI Architecture

## 1. AI Components

The platform has four major AI components:

1. Embeddings
2. RAG
3. Tools
4. Agent

---

## 2. Embeddings

Model:

```text
qwen3-embedding:4b
```

Purpose:

Convert text into numerical vectors.

Used for:

- Help article indexing
- User question embedding
- Semantic retrieval

---

## 3. LLM

Model:

```text
nemotron-3-super:cloud
```

Purpose:

- Generate support answers
- Reason over retrieved information
- Decide which authorized tools are useful
- Produce structured investigation results

---

## 4. RAG

RAG provides product knowledge.

```text
Question
   |
Embedding
   |
Vector Search
   |
Relevant Help Chunks
   |
Context
   |
Nemotron
   |
Answer + Sources
```

The model should not invent:

- Pricing
- Limits
- Product features
- Policies
- Incident information

when the required information is unavailable in trusted sources.

---

## 5. Tools

The initial tools are:

### Account Tool

Reads authorized account information.

### Entitlement Tool

Checks plan limits and current usage.

### Status Tool

Checks simulated service incidents.

---

## 6. Agent

The agent coordinates:

```text
Customer
   |
   v
Nemotron
   |
   +---- RAG
   |
   +---- Account Tool
   |
   +---- Entitlement Tool
   |
   +---- Status Tool
   |
   v
Investigation Result
```

---

## 7. Structured Agent Output

The final agent response should contain:

```json
{
  "answer": "...",
  "sources": [],
  "checksPerformed": [],
  "confidence": "high",
  "needsHuman": false,
  "recommendedAction": "..."
}
```

---

## 8. Authorization

The agent does not control permissions.

Instead:

```text
Agent requests tool
       |
       v
Backend validates identity
       |
       v
Backend validates authorization
       |
       v
Tool executes
```

---

## 9. Human Escalation

The agent can recommend escalation.

It cannot silently modify restricted business data.

```text
AI
 |
 +--> answer
 |
 +--> investigate
 |
 +--> recommend escalation
 |
 v
Human workflow
```

---

## 10. AI Safety Boundary

AI is primarily an:

```text
Investigation + Recommendation System
```

not an unrestricted:

```text
Business Action System
```

Human-controlled workflows remain responsible for sensitive subscription and billing actions.