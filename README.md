# Email to deal association

Middle-of-funnel prototype: take a small set of labeled emails and file each one on the right open deal, with a reason and a score.

This repo currently holds the data model, the flow explainer, and the implementation plan. The matching harness is not built yet.

- Canonical objects and CRM mapping: [schema/README.md](schema/README.md)
- Entity-relationship diagram: [schema/er.md](schema/er.md)
- JSON Schema: [schema/canonical.schema.json](schema/canonical.schema.json)
- Click-to-flip loop: [docs/flow.html](docs/flow.html)
- Next steps: [PLAN.md](PLAN.md).

## Why this exists

Sales lives in email. The CRM only knows a deal is alive if those emails land on the right opportunity. One account often has several open deals (new logo, renewal, expansion). The hard part is filing, not capturing.

Clay belongs in this loop as **who sent this / which company**. Open pipeline should stay on our side. See PLAN.md for the security constraint and the build sequence.
