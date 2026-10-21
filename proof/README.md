# What these six runs prove, and what they do not

On 2026-10-18 a test copy of `workflow.json` was saved to a self-hosted n8n 2.35.7 as "FREE-TEST Gmail invoice labels (Ollama)". Six sample emails were posted to it one at a time, and the model was a local Ollama `qwen2.5:3b`. The test copy, saved here as `test-copy.workflow.json`, makes three changes to the template. The Gmail Trigger is replaced by a webhook and a test-only Code node that rebuilds the trigger's output shape (Simplify off). That node turns each listed attachment into a small binary entry with a real file name and type, since file names are all the workflow reads from attachments. **Add Invoice Label** is disabled, so no Gmail account was touched. The sticky notes are removed. All six executions ended in `success`, and all six were sorted correctly. Three were labelled: the hosting invoice with `INV-20431.pdf` (kind `invoice`), the shop receipt with an amount paid (kind `receipt`), and the overdue-invoice reminder. The model called the reminder kind `invoice`, not `reminder`. It was still labelled, because both kinds get the label. Three were left alone: a quote with a PDF attached, a parcel delivery notice and a newsletter. The attachment file name went through to the last node in the runs that had one. **Not tested:** a real Gmail account (trigger, attachment download, adding a label), real PDFs, any model other than `qwen2.5:3b`, and non-English email. Each `run-N.json` is the full record of one execution.

| run | sample | result |
|---|---|---|
| 1 | hosting invoice, PDF attached | labelled, invoice, vendor CloudHost Billing |
| 2 | shop receipt, total paid | labelled, receipt, vendor Paper Store |
| 3 | quote for flyers, PDF attached | not labelled |
| 4 | parcel out for delivery | not labelled |
| 5 | design newsletter | not labelled |
| 6 | invoice 7781 overdue | labelled (model said kind invoice) |
