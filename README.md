# Free AI Document Processing with Claix BYOK

<div align="center">

# ✅ FREE WITH BYOK

## No Claix document-processing fee. Pay only your AI provider’s applicable token and model costs.

Process PDFs, Excel/CSV files, Word documents, images, HTML, XML, and text into structured JSON with Claix.

[Get started with Claix](https://claix.dev) · [View the A2A Agent Card](https://claix.dev/.well-known/agent-card.json)

</div>

---

> [!IMPORTANT]
> **“Free with BYOK” means Claix charges €0 in additional document-processing fees when you connect your own supported AI provider key.**  
> AI inference is not necessarily free: OpenAI, Gemini, Claude, Grok, or another provider may charge for tokens, vision/image usage, long context, caching, or premium models.

## What is this?

This repository shows how to process business documents with **Claix Bring Your Own Key (BYOK)**.

Claix turns documents and raw content into structured, automation-ready JSON. When you use your own supported AI provider key, Claix applies no additional processing fee.

```text
Your AI provider key
        ↓
Your chosen AI model
        ↓
Claix document-processing workflow
        ↓
Structured JSON for your app, agent, or automation

Claix BYOK processing fee: €0
AI provider usage: billed by your provider
```

## What can Claix process?

| Input | Typical use cases | Example structured output |
|---|---|---|
| PDF | Invoices, contracts, reports, purchase orders, forms, scanned PDFs | Invoice total, supplier, dates, clauses, IDs |
| Excel / CSV | Customer imports, supplier lists, financial exports, product data | Normalized records with consistent field names |
| Word / text documents | Contracts, proposals, policies, HR documents, notes | Parties, dates, obligations, extracted fields |
| Images | Receipts, photographed documents, screenshots, scanned forms | Merchant, date, amount, currency, reference |
| HTML | Email bodies, web pages, form submissions, exports | Leads, orders, tickets, structured records |
| XML | System exports, feeds, machine-generated business documents | Mapped fields for databases and workflows |
| Plain text | Messages, notes, logs, form content, copied text | Schema-validated JSON fields |

Claix is not limited to one document category. You can use the same workflow for invoices, contract review, onboarding, lead enrichment, receipts, procurement records, support files, HR documents, and internal operations.

## Why structured extraction?

Basic OCR reads text from an image or document.

Real business workflows need more than text.

They need to know:

- Which number is the final invoice total.
- Which date is the payment due date.
- Which company issued the invoice.
- Whether a contract has an automatic-renewal clause.
- Whether a required field is missing.
- Whether a value matches an internal database record.
- Whether a receipt should be accepted or reviewed.

Claix helps convert document content into fields your software can use.

```text
Document or raw content
        ↓
Claix extraction with your schema
        ↓
Structured JSON
        ↓
Validation rules
        ↓
Database, CRM, ERP, spreadsheet, agent, or automation
```

## Example: invoice PDF to JSON

Define the fields your workflow requires:

```json
{
  "invoice_number": "string",
  "supplier_name": "string",
  "invoice_date": "date",
  "due_date": "date",
  "total_amount": "number",
  "currency": "string",
  "purchase_order_number": "string"
}
```

Claix can return structured data in that format:

```json
{
  "invoice_number": "INV-2026-00456",
  "supplier_name": "Example Supplies Ltd",
  "invoice_date": "2026-03-14",
  "due_date": null,
  "total_amount": 1284.5,
  "currency": "EUR",
  "purchase_order_number": "PO-2049"
}
```

Your application can then apply deterministic business rules:

```text
If invoice_number is missing:
    Create a review task

If total_amount differs from the purchase order:
    Flag an exception

If all required fields are valid:
    Save the invoice to the accounting workflow
```

## How BYOK works

BYOK means **Bring Your Own Key**.

You connect your own AI provider account and choose the model used for document processing.

| With Claix BYOK | What it means |
|---|---|
| €0 Claix processing fee | Claix does not add a separate per-document processing charge |
| Your provider API key | You connect your own supported provider account |
| Your chosen model | You select a compatible model for the workflow |
| Direct provider billing | Your provider bills its own tokens and services |
| Claix workflow | You still receive schemas, structured JSON, document context, and automation-ready output |
| Provider control | You decide which provider account is billed for AI usage |

> [!TIP]
> BYOK is useful when you already have an OpenAI, Gemini, Claude, Grok, or other supported provider account and want direct control over model selection and AI-provider spending.

## BYOK setup

1. Create a workspace at [claix.dev](https://claix.dev).
2. Create or retrieve your Claix API key.
3. Open **LLM Keys** or the AI-provider settings in Claix.
4. Select **Bring Your Own Key (BYOK)**.
5. Choose a supported AI provider.
6. Add your provider API key.
7. Select a compatible model.
8. Create a schema for the fields you need.
9. Send a document to Claix.
10. Use the returned JSON in your workflow.

Your Claix API key authenticates your workspace.

Your BYOK provider key determines which external model performs the AI inference.

> [!WARNING]
> Never expose provider API keys in browser code, screenshots, public repositories, frontend applications, or logs. Store them in secure server-side environment variables or a secrets manager.

## Claix interfaces

Claix supports multiple ways to integrate document intelligence into an application or workflow.

| Interface | Best for | Endpoint |
|---|---|---|
| REST API | Backends, applications, serverless functions, and traditional integrations | See [Claix documentation](https://claix.dev) |
| MCP | IDEs, local AI tools, and agent frameworks that support MCP | `https://claix.dev/mcp` |
| A2A | Agents that need to discover and delegate document tasks to Claix | `https://claix.dev/a2a` |

## Use Claix as an A2A agent

Claix is a native Agent-to-Agent (A2A) document intelligence agent.

Other A2A-compatible agents, orchestrators, and scripts can discover Claix through its public Agent Card:

```text
[https://claix.dev/.well-known/agent.json](https://claix.dev/.well-known/agent.json)
```

Alternative Agent Card path:

```text
[https://claix.dev/.well-known/agent-card.json](https://claix.dev/.well-known/agent-card.json)
```

The A2A JSON-RPC endpoint is:

```text
[https://claix.dev/a2a](https://claix.dev/a2a)
```

### Discover the Agent Card

```bash
curl -sS "[https://claix.dev/.well-known/agent.json](https://claix.dev/.well-known/agent.json)"
```

### List schemas via A2A

```bash
curl -X POST "[https://claix.dev/a2a](https://claix.dev/a2a)" \
  -H "Content-Type: application/json" \
  -H "x-api-key: <YOUR_CLAIX_API_KEY>" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "message/send",
    "params": {
      "message": {
        "role": "user",
        "messageId": "list-schemas-example",
        "kind": "message",
        "parts": [
          {
            "kind": "data",
            "data": {
              "skill": "list-schemas"
            }
          }
        ]
      }
    }
  }'
```

Claix returns an A2A Task. The useful result is available in:

```text
result.artifacts[].parts[].data
```

For longer operations—such as PDF extraction, Agent Mode extraction, or Knowledge Space queries—Claix may return:

```text
status.state = "working"
```

You can then poll the task with `tasks/get` or register an A2A push-notification webhook.

## A2A skills

| Skill | What it does |
|---|---|
| `extract-excel` | Converts Excel or CSV files into schema-validated JSON |
| `extract-pdf` | Extracts structured information from a PDF |
| `extract-doc` | Extracts structured information from a text document |
| `extract-img` | Extracts structured information from an image |
| `extract-txt` | Extracts structured information from plain text, HTML, or XML |
| `get-document` | Retrieves raw content from a persisted document |
| `query-document` | Answers a focused question about a persisted document |
| `create-space` | Creates a Knowledge Space for related documents |
| `query-space` | Answers questions across documents in a Knowledge Space |
| `delete-space` | Deletes a Knowledge Space |
| `delete-document` | Deletes a persisted document |
| `list-schemas` | Lists schemas in the current account |
| `create-schema` | Creates an extraction schema |
| `delete-schema` | Deletes an extraction schema |
| `agent-extract-*` | Uses Agent Mode reasoning for supported extraction types |

The current parameters, example payloads, and skill definitions are available in the [Claix A2A Agent Card](https://claix.dev/.well-known/agent-card.json).

## Document context

Document extraction does not have to be a one-time operation.

After processing a document, you can retain it as document context and ask focused questions later without re-uploading the full file.

```text
Process a document once
        ↓
Persist document context
        ↓
Ask a follow-up question later
```

Example questions:

```text
What is the early termination penalty?

Does this agreement renew automatically?

What payment terms are listed on this invoice?

Which party is responsible for maintenance?
```

Using persisted document context can reduce repeated uploads and avoid sending the same full document to a model every time you need an answer.

## Knowledge Spaces

Knowledge Spaces group related documents and let you query them together.

```text
Supplier contract
    + invoice
    + purchase order
    + price list
        ↓
Claix Knowledge Space
        ↓
Cross-document question
```

Example questions:

```text
Which invoice does not match the contracted price?

What is the total billed by this supplier?

Which contracts are due to renew soon?

Do any purchase orders exceed their approved amount?
```

## Automation example

Claix can sit inside an automation workflow:

```text
New email attachment
        ↓
Download PDF
        ↓
Send PDF to Claix with an invoice schema
        ↓
Receive structured JSON
        ↓
Validate required fields
        ↓
Write data to database or accounting system
        ↓
Notify finance only when review is required
```

You can connect Claix with:

- n8n
- Make
- Zapier
- Supabase
- Serverless functions
- Custom backends
- CRM systems
- ERP systems
- Databases
- Internal tools
- Customer portals
- Cloud-storage workflows
- A2A-compatible agents

## Practical examples

### Invoice processing

```text
Invoice PDF
        ↓
Extract supplier, invoice number, tax, total, currency, and due date
        ↓
Compare against purchase order
        ↓
Approve matching invoices
        ↓
Route exceptions to finance review
```

### Contract extraction

```text
Contract upload
        ↓
Extract parties, dates, renewal terms, payment conditions, and obligations
        ↓
Store data in a contract-management system
        ↓
Create reminders for renewal and expiration dates
```

### Spreadsheet normalization

```text
Excel or CSV upload
        ↓
Map inconsistent column names to a standard schema
        ↓
Return normalized JSON records
        ↓
Import into CRM, database, or internal system
```

### Receipt processing

```text
Receipt image
        ↓
Extract merchant, date, total, and currency
        ↓
Validate expense rules
        ↓
Approve or send for manual review
```

## Managed AI vs BYOK

| Option | Best for | Billing |
|---|---|---|
| Claix Managed AI | Teams that want the simplest setup and do not want to manage external provider keys | Claix-managed processing pricing |
| Bring Your Own Key | Teams that want provider choice, model control, and no additional Claix processing fee | Your AI provider bills its applicable token and service usage |

Use Managed AI when you want Claix to handle model configuration.

Use BYOK when you already have a provider account, want to choose a compatible model, or want direct control over AI-provider billing.

## Important limitations

- BYOK removes the additional Claix document-processing fee; it does not remove your AI provider’s charges.
- Output quality depends on document quality, input complexity, selected model, schema design, and provider behavior.
- Validate critical values such as monetary amounts, dates, IDs, legal terms, and compliance-related fields.
- Use human review for high-impact decisions, payments, contracts, employment, healthcare, security, or compliance workflows.
- Avoid sending sensitive documents unless your security, privacy, retention, and provider agreements support the intended use.
- Do not commit API keys, private documents, customer data, or credentials to this repository.

## FAQ

### Is Claix a free PDF-to-JSON API?

With BYOK enabled, Claix can process PDFs into structured JSON with no additional Claix processing fee. You pay your selected AI provider for any applicable model, token, image, or service usage.

### Can I process scanned PDFs for free?

You can process scanned PDFs through Claix with BYOK without a Claix processing fee. Your AI provider may charge for vision or model usage.

### Is Claix only OCR?

No. OCR reads text. Claix is designed to extract structured fields with business meaning, such as invoice totals, supplier names, dates, IDs, contract terms, and normalized records.

### Can I use my own OpenAI, Gemini, Claude, or Grok key?

Yes. BYOK supports connecting your own key from supported AI providers and selecting a compatible model. Check the current Claix settings and documentation for supported providers and models.

### Do I pay Claix when I use BYOK?

Claix does not charge an additional BYOK document-processing fee. You remain responsible for charges from your AI provider.

### Can Claix process Excel and CSV files?

Yes. Claix can process Excel and CSV files and return structured data based on the schema your workflow requires.

### Can Claix process Word documents?

Yes. Claix supports text-document extraction for Word-compatible and text-based document workflows.

### Can I use Claix with n8n?

Yes. You can call Claix from n8n through HTTP/API steps and use the resulting structured JSON to update databases, CRMs, notifications, spreadsheets, and approval workflows.

### Does Claix replace a database, CRM, or ERP?

No. Claix extracts and structures document content. Your application, database, CRM, ERP, or automation platform stores the data and executes the business workflow.

### Does Claix support AI agents?

Yes. Claix supports REST, MCP, and native A2A. A2A-compatible agents can discover Claix through its public Agent Card and delegate document-processing tasks to the A2A endpoint.

## Documentation

- [Claix website](https://claix.dev)
- [Claix A2A Agent Card](https://claix.dev/.well-known/agent-card.json)
- [Claix A2A endpoint](https://claix.dev/a2a)
- [Claix MCP endpoint](https://claix.dev/mcp)
- [A2A protocol](https://a2a-protocol.org/)

## Contributing

Contributions are welcome, especially:

- Invoice, contract, receipt, and spreadsheet examples
- A2A integration examples
- n8n, Make, Zapier, and serverless workflows
- Schema examples for common document types
- Improvements to accuracy, clarity, and security guidance

Please never commit real API keys, provider credentials, private documents, or customer data.

## License

This repository is licensed under the [MIT License](LICENSE).

## Disclaimer

This is an independent educational and integration repository. Claix is a product and service of its respective owners. Check the current Claix documentation, product terms, privacy information, and your AI provider’s pricing before using any workflow in production.
