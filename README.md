# Email Intelligence Framework

**Email Intelligence — Structured Data from Unstructured Inputs**

---

Three workflows, one theme: taking unstructured inputs and producing structured, actionable outputs.

**Email classification and routing**

The business inbox receives four types of messages: invoices, compliance notices, product/service updates, and noise. A Gmail → n8n connection reads incoming messages. Claude classifies each one. Four routing rules handle the rest: invoices go to the invoice extraction handler, compliance notices trigger a Slack flag for immediate review, product updates log to the email registry, junk archives without action.

The classifier doesn't use keywords. It reads the email and makes a judgment. The rules determine what happens next. That separation — classify once, route by rule — means adding a new category is one rule addition, not a rewrite of the classifier.

**Invoice extraction**

Invoices that reach the handler get parsed: vendor name, amount, invoice date, due date, category. Claude extracts the fields. A confidence threshold determines whether the record goes straight to the expenses table or to a pending queue for human review. Slack confirmation closes the loop — I know it landed and whether it needs attention.

The constraint that shaped the design: the AI doesn't approve financial records. It extracts, categorizes, and routes. The human reviews the pending queue and clears it. Automation handles 90%+ of invoices without intervention. The 10% that need a second look are flagged, not silently filed.

**Why it matters:** Extracting structured signal from unstructured inputs is the same problem across both workflows. The patterns — classify before routing, separate extraction from decision-making, automate preparation while keeping human approval at the right boundary — apply anywhere you're processing documents, emails, or records at volume.
