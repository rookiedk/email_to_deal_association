# Entity-relationship schema

```mermaid
erDiagram
  ACCOUNT ||--o{ CONTACT : "has"
  ACCOUNT ||--o{ DEAL : "owns"
  CONTACT ||--o{ DEAL_CONTACT_ROLE : "plays"
  DEAL ||--o{ DEAL_CONTACT_ROLE : "includes"
  CONTACT ||--o{ EMAIL : "may send"
  EMAIL ||--o| EMAIL_ASSOCIATION : "filed as"
  DEAL ||--o{ EMAIL_ASSOCIATION : "receives"
  ACCOUNT ||--o{ EMAIL_ASSOCIATION : "context"
  CONTACT ||--o{ EMAIL_ASSOCIATION : "resolved sender"
  EMAIL ||--o| GOLD_LABEL : "scored against"
  EMAIL ||--o{ AUDIT_EVENT : "explains"

  ACCOUNT {
    string id PK
    string name
    string domain
    string aliases
  }

  CONTACT {
    string id PK
    string account_id FK
    string email
    string full_name
    string title
  }

  DEAL {
    string id PK
    string account_id FK
    string name
    number amount
    string stage
    boolean is_closed
    string type
  }

  DEAL_CONTACT_ROLE {
    string deal_id FK
    string contact_id FK
    string role
    boolean is_primary
  }

  EMAIL {
    string id PK
    string thread_id
    string from_email
    string subject
    datetime sent_at
  }

  EMAIL_ASSOCIATION {
    string email_id FK
    string deal_id FK
    string account_id FK
    string contact_id FK
    number confidence
    string decision
    string source
  }

  GOLD_LABEL {
    string email_id PK
    string contact_id FK
    string account_id FK
    string deal_id FK
  }

  AUDIT_EVENT {
    string id PK
    string email_id FK
    string clay_run_id
    string chosen_deal_id
    string evidence
  }
```

Cardinality that matters for matching:

- One account, several open deals (renewal vs expansion) is the core ambiguity.
- Several contacts on one deal is normal; the sender may not be the primary contact.
- An email with no gold deal is a true negative, not a missing label.
