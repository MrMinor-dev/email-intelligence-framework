# Email Intelligence Framework

**Email Intelligence — Structured Data from Unstructured Inputs**

---

Every business has the same inbox problem: invoices that need to be logged, compliance notices that need immediate attention, vendor updates worth tracking, and noise that should disappear. The default solution is a human reading every email and deciding what to do. This automates that decision — and the actions that follow.

**Email Router** (`aCEsXeWi75h6ffV7` — 15 nodes) processes every incoming message. Claude reads the email and classifies it into one of 9 categories. Rules handle the rest — no human triage required for routine volume:

```
Gmail Trigger ─┐
               ├─ V4 Rules Classifier → Log to Database → Mark as Read
Webhook       ─┘                                               └─ Has Label? → Apply Gmail Label
                                                                                    └─ Should Archive?
                                                                                         ├─ [yes] → Archive Email
                                                                                         └─ [no]  → Keep in Inbox
                                                                                                        └─ Log Health Check
Error Trigger → Error: Log
```

Error paths from three nodes merge into one table — any failure in the flow is captured regardless of where it happened.

**Invoice Handler** (`SzwzUJfjjNo6qJyn` — 16 nodes) runs daily at 6 AM on whatever the Router flagged as financial. It handles the two formats an invoice arrives in — email body or PDF attachment — and produces the same structured output either way:

```
Daily 6am / Chat Trigger
  └─ Get Financial Emails → Any Emails?
                                 ├─ [no]  → Done
                                 └─ [yes] → Extract Invoice Data → Has PDF?
                                                                        ├─ [yes] → Download PDF → Upload to Drive ─┐
                                                                        └─ [no]  → Continue ─────────────────────┤
                                                                                                           Merge Back
                                                                                                                └─ Insert to expenses → Apply Label → Slack Summary
```

Vendor name, amount, invoice date, due date, IRS category — extracted and written to `aos_expenses` automatically. Slack summary at the end closes the loop: how many invoices processed, whether anything needs review.

The classifier reads the email rather than matching keywords. That separation — classify once, route by rule — means adding a new email type is one rule addition, not a classifier rewrite. 90%+ of emails process without intervention.

**Why it matters:** Most businesses are one step away from this — Gmail is already connected, the emails are already arriving. The gap is structured extraction and routing logic. The same architecture applies anywhere unstructured inputs need to become actionable records: support tickets, vendor communications, compliance filings.
