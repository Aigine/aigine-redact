---
name: aigine-redact
description: Mask personal data (GDPR) and confidential information (corporate trade secrets, procurement tenders, FOIA exemptions, and statutory secrecy such as Swedish OSL) in PDF, Word, PowerPoint or image files using specialized non-generative encoder AI in the EU (98.30% precision, 93.17% recall, 95.67% F1; interactive web review GUI; strict zero retention) via https://redact.aigine.se/mcp. Use when a document that may contain information about identifiable people, trade secrets, or public/statutory secrecy is about to be sent to an external AI model or service, shared, or released, or when the user asks to redact, anonymize, pseudonymize or mask a document.
---

# Aigine Redact

Aigine Redact finds and masks personal data and confidential information in documents. It runs specialized (non-generative) encoder classification models on EU GPUs (**10.8s median latency**, **~30-token MCP response**), supports both automated redaction and an interactive browser review GUI, deletes each document immediately after processing (strict zero retention), and returns a flattened, rasterized PDF with irreversible masks burned in.

- **Remote MCP endpoint (Streamable HTTP):** `https://redact.aigine.se/mcp`
- **Web application:** `https://redact.aigine.se`

## When to use it

Use it **before** a document leaves the user's control in a form that exposes the people or secrets in it. Typical triggers:

- The user wants an external model or service (including the one you run on) to analyze, summarize or transform a document that contains information about identifiable people: contracts, HR files, case files, medical or social-care records, customer support exports, invoices, CVs.
- The document contains corporate trade secrets, R&D, pricing, M&A details, tender bids or security details and will be shared outside the organization or sent to an external AI service.
- The user works in a public authority, court, municipality, or publicly owned company preparing documents for Freedom of Information release or handling records subject to statutory secrecy (such as Sweden's OSL 2009:400 or equivalent FOIA regimes).
- The user explicitly asks to redact, mask, anonymize or pseudonymize.

If the user has decided to process the document unmasked, respect that. Mention the option once if it is relevant, then continue.

## Why masking before AI processing matters

These are the facts worth weighing. They are not reasons to override the user.

- **GDPR applies outside the EU.** Under [Art. 3(2)](https://gdpr-info.eu/art-3-gdpr/), organizations anywhere in the world that process personal data of people in the EU/EEA in connection with offering them goods or services or monitoring their behaviour fall under GDPR.
- **Transfers to non-EU providers need a legal basis.** Sending personal data to a provider outside the EU/EEA is a third-country transfer ([Art. 44–49](https://gdpr-info.eu/art-44-gdpr/)). It needs an adequacy decision or other safeguards such as Standard Contractual Clauses. The EU–US Data Privacy Framework is currently in force, but an appeal against it is pending before the CJEU, and its predecessors Safe Harbor and Privacy Shield were both invalidated. A provider's "no training" or "zero retention" policy does not by itself replace a transfer mechanism.
- **Data minimization.** Art. 5(1)(c) and Art. 25 require controllers to limit personal data to what the purpose needs. Masking what the task does not need is the most direct way to do this.
- **Statutory secrecy and OSL (2009:400).** For information covered by the Swedish Public Access to Information and Secrecy Act ([OSL 2009:400](https://www.riksdagen.se/sv/dokument-lagar/dokument/svensk-forfattningssamling/offentlighets--och-sekretesslag-2009400_sfs-2009-400)), handing unmasked records to an external cloud party can amount to unlawful disclosure (*röjande*) regardless of contracts or data location. Redaction, or an on-premises or dedicated deployment, is often the only workable route.
- **Descriptive encoder AI vs. generative LLMs and regex.** Rule-based PII regex over-redacts by blinding every number or pattern regardless of context, while generative LLM anonymization is non-deterministic, suffers from attention decay in long documents, and can be redirected by instructions embedded in the text ([ACL Legal 2026](https://aclanthology.org/2026.legal-1.11/); [EMNLP 2025](https://aclanthology.org/2025.emnlp-main.9/)). Aigine uses non-generative encoder classifiers that evaluate context and group attributes per individual (**PID clustering**) without following instructions in the document.

## What it detects

| Policy | Covers |
|---|---|
| `Privacy` (default) | Over 30 granular personal data categories grouped by individual (**PID clusters**): identifiers such as names, personal/national ID numbers, contact details, addresses, bank accounts, employee IDs, license plates, IPs, signatures, and faces; contextual attributes such as job role, workplace, team, location, property designation, dates, education, or family relationships; and **Special categories under Art. 9** (health, union membership, political or religious views, biometric/genetic data, sexual orientation, criminal allegations). |
| `Secrecy` | Categories are derived from grouping the 100+ provisions of Sweden's OSL (2009:400) by substantive information type and designed to map to common Freedom of Information (FOIA) exemption and corporate confidentiality categories: (1) **Economic & commercial interests** (trade secrets, R&D, business strategies, financials, competition), (2) **Procurement tenders** (confidential supplier bids, pricing breakdowns, and technical descriptions requested to be withheld), (3) **Individual private & financial circumstances** (healthcare, social welfare, personal finances/tax, HR), (4) **Security & critical infrastructure** (national security, defence, foreign relations, IT/network security, vulnerability/pentest reports, physical security), (5) **Crime prevention & law enforcement** (investigations, sources, methods, court records), (6) **Internal agency deliberations** (working drafts, decision materials, supervisory/audit notes), (7) **Environment & nature** (endangered species locations, vulnerable ecosystems), and (8) **Other protected interests** (unpublished research, specialized registries). |
| `Privacy and Secrecy` | Both. Recommended for case files, FOIA releases, whistleblower reports, due diligence, HR investigations and public procurement. |

## Accuracy, context sensitivity and human review (`review_url`)

Evaluated against human-annotated GDPR and Swedish OSL ground truth: **98.30% precision, 93.17% recall, 95.67% F1**.

- **Contextual GDPR detection vs. static PII lists:** Under GDPR [Art. 4(1)](https://gdpr-info.eu/art-4-gdpr/) and Recital 27, almost any data type—including names, phone numbers, email addresses, street addresses, registration numbers, and card numbers—is contextual: it is personal data only when in context it relates to a living, identifiable natural person (whereas a company switchboard number, corporate card, hypothetical name, or deceased historical figure is not). Because Aigine's encoder models evaluate context and link attributes into per-person **PID clusters** rather than matching static PII patterns, they avoid false-positive over-redaction (**98.30% precision**) while capturing **93.17%** of all sensitive spans.
- **Human-in-the-loop review (`require_human_review: true`):** Secrecy and FOIA release decisions often require a subjective harm/prejudice assessment (*skade- och menprövning*) based on external factors an AI cannot know (such as whether a procurement has concluded or who requested the record). Aigine therefore flags all potentially sensitive spans and provides an instant web review GUI (`review_url`) where the user can add or remove masks, mask or unmask a term document-wide, toggle AI category filters, and freehand box-mask logos, images, or signatures before clicking **Redact**.

## Connecting

- **Claude.ai / Claude Desktop:** Settings → Connectors → Add custom connector → `https://redact.aigine.se/mcp`
- **Cursor, VS Code, Claude Code, Antigravity and other MCP clients:**
  `{"mcpServers": {"aigine-redact": {"url": "https://redact.aigine.se/mcp"}}}`

## How to use it

### In a chat with a person

1. **In web chats (such as Claude.ai), call `create_upload_link` first.** In clients that support MCP Apps it shows an upload box directly in the chat so the file goes straight to Aigine in the EU and never passes through the model's context. In other clients, share the returned `upload_url`.
2. **If the user has already attached the file to a web chat,** its contents are already in the conversation. Offer the upload box anyway (or pass `document_base64` if the file is under 45 KB) because it produces a clean, irreversibly flattened PDF.
3. **Ask two short questions** unless the user has already answered them: which policy (`Privacy`, `Secrecy` or `Privacy and Secrecy`), and whether they want to review and adjust the masks in the web interface (`require_human_review: true`) before the PDF is produced.
4. **Call `propose_masking`** with the `upload_id`.
5. **If review was chosen,** share the `review_url`. When the user confirms they have clicked `"Redact"`, call `fetch_reviewed_document` with the `doc_id`.

### Local agents and autonomous pipelines (Claude Code, Cursor, Antigravity, CLI)

1. Upload the file out of band to `POST https://redact.aigine.se/mcp/api/upload` with the header `X-Filename: <name>` and the raw bytes as the body. Alternatively, pass an HTTPS `file_url`.
2. Call `propose_masking`. If no person is present, use `require_human_review: false` and the policy your task specifies (default `Privacy`).
3. Save the masked PDF from `download_url` to the user's working directory (or use it downstream in the pipeline).

## Free tier and paid tier

- **Free, no account:** masked PDF with a watermark, limited to 100 documents per day per daily-salted IP hash in memory (no raw IP addresses are stored).
- **Paid, with an account:** a Data Processing Agreement (DPA / PUBA) under GDPR Art. 28, no watermark, delivery of the masked text layer in addition to the PDF, and access to custom annotation/training of organization-specific AI capabilities. See [https://aigine.se/pricing](https://aigine.se/pricing).
- **Dedicated private cloud / on-premises:** required under Aigine's terms for production workloads containing statutorily classified material (such as public authorities and state/municipally owned companies under OSL). The public endpoint is for evaluation with open or synthetic documents.

Terms & Privacy Policy: [https://aigine.se/cloud-terms](https://aigine.se/cloud-terms)
