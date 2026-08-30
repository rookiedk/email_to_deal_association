# Canonical CRM model

This is the standardized data model the matcher reads and writes. Field names are ours. Salesforce and HubSpot column names map onto them; they are not copied 1:1.

Objects:

| Object | Purpose |
| --- | --- |
| `account` | Company / customer. |
| `contact` | Person, usually at one account. |
| `deal` | Open or closed opportunity on an account. |
| `deal_contact_role` | Who is on a deal, and in what role. |
| `email` | Inbound or outbound message. |
| `email_association` | Predicted or confirmed filing of an email onto CRM records. |
| `gold_label` | Held-out correct filing, used only for eval. |
| `audit_event` | Why a filing was made. |

JSON Schema: [canonical.schema.json](canonical.schema.json). Diagram: [er.md](er.md).

## Relationship rules

- One account has many contacts and many deals.
- A contact belongs to one account in v1 (no multi-account contacts).
- A deal belongs to one account.
- Contacts and deals are many-to-many through `deal_contact_role` (champion, economic buyer, evaluator, legal, other).
- An email can associate to at most one deal in v1. It may also point at a contact and account even when no deal is chosen.
- `email_association.deal_id` is null when the decision is `unmatched` or the gold says there is no deal.
- Closed deals stay in the catalog for traps and history. The matcher only auto-files onto **open** deals.

## CRM mapping

| Canonical | Salesforce | HubSpot |
| --- | --- | --- |
| `account` | Account | Company |
| `contact` | Contact | Contact |
| `deal` | Opportunity | Deal |
| `deal.amount` | Amount | amount |
| `deal.stage` | StageName | dealstage |
| `deal.is_closed` | IsClosed | derived from stage |
| `deal.close_date` | CloseDate | closedate |
| `contact.email` | Email | email |
| `account.domain` | Website (host) | domain |
| `deal_contact_role` | OpportunityContactRole | deal-contact association + role label |
| `email` | EmailMessage / Activity | Engagement (email) |
| `email_association` | Task.WhatId / EmailMessageRelation | engagement associations |

Clay never sees `deal` or `deal_contact_role`. It only returns person/company attributes that we join onto `contact` and `account`.
