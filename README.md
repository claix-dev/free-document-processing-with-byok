# Free AI Document Processing with Claix BYOK

> Extract structured data from PDFs, Excel/CSV files, Word documents, images, HTML, XML, and text with Claix. With Bring Your Own Key (BYOK), Claix charges **€0 in document-processing fees**—you pay only your AI provider's model and token costs.

[![Claix](https://img.shields.io/badge/Claix-Document%20Intelligence-6D28D9?style=flat-square)](https://claix.dev)
[![A2A](https://img.shields.io/badge/A2A-Compatible-0F766E?style=flat-square)](https://claix.dev/.well-known/agent-card.json)
[![BYOK](https://img.shields.io/badge/BYOK-No%20Claix%20processing%20fee-16A34A?style=flat-square)](https://claix.dev)
[![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)](LICENSE)

## Quick answer: is Claix free?

**Yes, from the Claix platform side when you use BYOK.** Connect your own supported AI provider key, select a model, and Claix applies **no additional document-processing fee**.

You still pay your selected provider for any applicable model usage, including input/output tokens, image or vision processing, long-context use, caching, or premium-model charges.

```text
Your AI provider key
        ↓
Your selected model
        ↓
Claix document-processing workflow
        ↓
Structured JSON for your app or automation

Claix BYOK processing fee: €0
AI provider usage: billed by your provider
```

> [!IMPORTANT]
> BYOK does **not** mean AI inference is free. It means Claix does not add an extra processing charge when you use your own provider key.

## What this repository covers

This repository shows how to use Claix with BYOK to process business documents and produce structured, automation-ready output.

- Extract fields from PDFs, invoices, contracts, reports, and forms.
- Convert Excel and CSV data into normalized JSON.
- Extract data from Word documents, text files, images, HTML, and XML.
- Define a schema so every document returns predictable fields.
- Use document context without repeatedly uploading the same file.
- Query a Knowledge Space across multiple related documents.
- Integrate document processing into backends, AI agents, and workflows.
- Call Claix through REST, MCP, or its native A2A endpoint.

## Supported inputs

| Input | Typical use cases | Example output |
|---|---|---|
| PDF | Invoices, contracts, purchase orders, reports, forms, scanned PDFs | Invoice number, total, supplier, dates |
| Excel / CSV | Customer imports, product lists, supplier data, financial exports | Normalized records with consistent keys |
| Word / text | Contracts, proposals, policies, HR documents, notes | Parties, clauses, dates, obligations |
| Images | Receipts, photographs, screenshots, scanned forms | Merchant, total, currency, reference |
| HTML | Email content, web pages, forms, exported records | Structured lead, ticket, or order data |
| XML | System exports, feeds, business documents | Mapped fields for applications or databases |
| JSON | Structured records that need spreadsheet output | Generated Excel file |

## Why structured extraction matters

Basic OCR can retrieve text. But document workflows usually need more than words.

For example, an accounts-payable system needs to know:

- Which number is the invoice total.
- Which date is the payment due date.
- Who issued the invoice.
- Whether a purchase-order reference is present.
- Whether required data is missing.
- Whether the extracted amount matches an internal record.

Claix helps transform document content into structured fields with business meaning.

```text
Invoice PDF
    ↓
Claix extraction with your schema
    ↓
Structured JSON
    ↓
Validation and business rules
    ↓
Database, CRM, ERP, spreadsheet, or approval workflow
```

## Example: invoice PDF to JSON

Define the fields required by your workflow:

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

Claix can return a structured result like this:

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

Then your application or automation can apply deterministic rules:

```text
If invoice_number is missing:
    Create a review task

If total_amount differs from the purchase order:
    Flag an exception

If all required fields are valid:
    Save the invoice to the accounting workflow
```

## BYOK setup

To use Claix with no additional Claix processing fee:

1. Create a Claix workspace at [claix.dev](https://claix.dev).
2. Create or retrieve your Claix API key.
3. Open the **LLM Keys** or AI-provider settings in Claix.
4. Select **Bring Your Own Key (BYOK)**.
5. Choose a supported AI provider.
6. Add your provider API key.
7. Select a compatible model.
8. Create a schema for the fields you want to extract.
9. Send a document to Claix.
10. Use the returned structured JSON in your application or workflow.

Your Claix API key authenticates access to your Claix workspace. Your BYOK provider key determines the external model that performs AI inference.

> [!TIP]
> Test the workflow using a non-sensitive sample invoice, contract, spreadsheet, or receipt before connecting production systems.

## Choose an integration interface

Claix exposes the same document-intelligence capabilities through three different interfaces.

| Interface | Best for | Endpoint |
|---|---|---|
| REST API | Backends, serverless functions, traditional applications | See [Claix documentation](https://claix.dev) |
| MCP | IDEs, local AI tools, and agent frameworks that support MCP | `https://claix.dev/mcp` |
| A2A | Agents that need to discover and delegate document tasks to Claix | `https://claix.dev/a2a` |

## A2A: call Claix from another agent

Claix is a native Agent-to-Agent (A2A) document intelligence agent.

An A2A-compatible agent can discover Claix through its public Agent Card:

```text
[https://claix.dev/.well-known/agent.json](https://claix.dev/.well-known/agent.json)
```

Alternative Agent Card path:

```text
[https://claix.dev/.well-known/agent-card.json](https://claix.dev/.well-known/agent-card.json)
```

Then send JSON-RPC 2.0 requests to:

```text
[https://claix.dev/a2a](https://claix.dev/a2a)
```

### Discover Claix

```bash
curl -sS "[https://claix.dev/.well-known/agent.json](https://claix.dev/.well-known/agent.json)"
```

### List schemas through A2A

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

Claix returns an A2A Task. Read the result from:

```text
result.artifacts[].parts[].data
```

For long-running work such as PDF extraction or cross-document queries, Claix may return:

```text
status.state = "working"
```

You can then poll with `tasks/get` or register an A2A push-notification webhook.

## A2A skills

| Skill | What it does |
|---|---|
| `extract-excel` | Converts Excel or CSV data into schema-validated JSON |
| `convert-json-to-excel` | Generates an Excel file from JSON documents |
| `extract-pdf` | Extracts structured information from a PDF |
| `extract-doc` | Extracts structured information from a text document |
| `extract-img` | Extracts structured information from an image |
| `extract-txt` | Extracts structured information from plain text, HTML, or XML |
| `get-document` | Retrieves raw content from a persisted document |
| `query-document` | Answers a focused question about a persisted document |
| `create-space` | Creates a document Knowledge Space |
| `query-space` | Answers questions across documents in a Knowledge Space |
| `delete-space` | Deletes a Knowledge Space |
| `delete-document` | Deletes a persisted document |
| `list-schemas` | Lists schemas in the current account |
| `create-schema` | Creates an extraction schema |
| `delete-schema` | Deletes an extraction schema |
| `agent-extract-*` | Uses Agent Mode reasoning for supported extraction types |

The Agent Card contains the current skill definitions, required parameters, and request examples.

## Document context

Document processing does not need to stop after the initial extraction.

After processing a document, you can use document context to ask follow-up questions without sending the entire file again.

```text
Process a contract once
    ↓
Persist document context
    ↓
Ask a targeted question later
```

Examples:

```text
What is the early termination penalty?

Does this agreement renew automatically?

What payment terms are listed?

Which party is responsible for maintenance?
```

This can reduce repeated uploads and avoid sending the same full document to the model for every question.

## Knowledge Spaces

A Knowledge Space groups related documents so you can query them together.

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

Examples:

```text
Which invoice does not match the contracted price?

What is the total billed by this supplier?

Which contracts renew in the next 90 days?

Do any purchase orders exceed their approved amount?
```

## Automation example

A document-driven automation can look like this:

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
Write to database or accounting system
    ↓
Notify finance only when review is needed
```

Claix can be connected to:

- n8n.
- Make.
- Zapier.
- Supabase.
- Serverless functions.
- Custom backends.
- CRM systems.
- ERP systems.
- Databases.
- Internal tools.
- Customer portals.
- Cloud-storage workflows.
- A2A-compatible agents.

## Example use cases

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
Route exceptions to finance
```

### Contract extraction

```text
Contract upload
    ↓
Extract parties, dates, renewal terms, payment conditions, and obligations
    ↓
Store structured data in a contract-management system
    ↓
Create reminders for expiration or renewal dates
```

### Spreadsheet normalization

```text
Excel or CSV upload
    ↓
Map varying column names into a standard schema
    ↓
Return consistent JSON records
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
| Claix Managed AI | Teams that want the simplest configuration and do not want to manage external provider keys | Claix-managed processing pricing |
| Bring Your Own Key | Teams that want provider choice, model control, and no additional Claix processing fee | Your AI provider bills token and service usage |

Use Managed AI when you want Claix to manage model configuration.

Use BYOK when you already have a provider account, want to select your own compatible model, or need direct control over AI-provider billing.

## Important limitations

- BYOK removes the additional Claix document-processing fee; it does not remove AI-provider costs.
- Output quality can vary based on document quality, selected model, schema design, input complexity, and provider behavior.
- Validate critical fields such as monetary amounts, dates, identifiers, legal terms, and compliance-related information.
- Add human review for high-impact decisions, payments, legal approvals, healthcare, employment, security, or other consequential workflows.
- Do not expose your provider API key in browser code, public repositories, screenshots, client-side applications, or logs.
- Use server-side secrets management and limit keys to the smallest permissions and spending limits your provider allows.

## FAQ

### Is Claix a free PDF-to-JSON API?

Claix can convert PDFs into structured JSON. With BYOK enabled, Claix charges no additional processing fee; you pay your selected AI provider for applicable model usage.

### Can I process scanned PDFs with BYOK?

Yes. Claix can process scanned PDFs through BYOK. Your provider may charge for vision or model usage, but Claix does not add a separate BYOK document-processing fee.

### Is Claix only OCR?

No. OCR extracts text. Claix is designed to convert document content into structured fields that your software can use, such as invoice totals, supplier names, dates, IDs, contract clauses, and normalized records.

### Can I use my own OpenAI, Gemini, Claude, or Grok key?

Yes. BYOK is designed for supported external AI providers and compatible models. Check the Claix settings or documentation for the current provider and model availability.

### Do I pay Claix when using BYOK?

Claix does not charge an additional BYOK processing fee. You remain responsible for the costs charged by your AI provider.

### Can Claix process Excel and CSV files?

Yes. Claix can process Excel and CSV inputs and return normalized, structured data based on the schema your workflow requires.

### Can Claix process Word documents?

Yes. Claix supports text-document extraction for Word-compatible and text-based document workflows.

### Can I use Claix with n8n?

Yes. You can call Claix from n8n through HTTP/API steps and use the returned JSON to update a database, CRM, spreadsheet, notification, or review workflow.

### Does Claix replace my database, CRM, or ERP?

No. Claix processes and structures document content. Your application, database, CRM, ERP, or automation system remains responsible for storing data and executing your business workflow.

### Does Claix support AI agents?

Yes. Claix supports REST, MCP, and native A2A. A2A-compatible agents can discover Claix through its Agent Card and delegate document-processing tasks through the A2A endpoint.

## Documentation

- [Claix website](https://claix.dev)
- [Claix A2A Agent Card](https://claix.dev/.well-known/agent-card.json)
- [Claix A2A endpoint](https://claix.dev/a2a)
- [Claix MCP endpoint](https://claix.dev/mcp)
- [A2A protocol](https://a2a-protocol.org/)

## Contributing

Contributions are welcome, especially:

- Example workflows for invoices, contracts, receipts, and spreadsheet normalization.
- A2A integration examples.
- n8n, Make, Zapier, and serverless examples.
- Schema examples for common document types.
- Improvements to clarity, accuracy, and security guidance.

Please do not commit real API keys, provider keys, private documents, customer data, or credentials.

## License

This repository is licensed under the [MIT License](LICENSE).

## Disclaimer

This is an independent educational and integration repository. Claix is a product and service of its respective owners. Review the current Claix documentation, product terms, privacy documentation, and AI-provider pricing before using it in production.
