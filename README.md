# Label invoice and receipt emails in Gmail with a local Ollama model

A free n8n workflow. Every new email in your Gmail inbox is checked, and the ones that ask for a payment or confirm one get a Gmail label you choose, such as "Invoices". At month end, one click on that label shows everything you need for your bookkeeping.

The check runs on your own machine through [Ollama](https://ollama.com), so there is no API bill and no email text goes to an AI company.

**Download:** [gmail-invoice-labels-ollama.zip](https://github.com/TuckerCode/n8n-gmail-invoice-labels-ollama/releases/latest/download/gmail-invoice-labels-ollama.zip) (the workflow, this README and six saved test runs). The same files are in this repository.

## What it does

1. Checks your Gmail inbox every minute for new emails (your own sent mail is left out) and downloads their attachments.
2. Sends the sender, subject, attachment file names and the first 3,000 characters of the body to Ollama. The attachment contents are not read.
3. Asks the model whether the email is an invoice, receipt, bill or payment reminder, and who sent it. Quotes, estimates, newsletters, marketing, shipping updates with no amount and login emails count as not invoices.
4. Adds your label only when the model says yes and names one of those four kinds. If its answer cannot be read, no label is added.
5. Passes the downloaded attachments along with each labelled email, so you can add one node to save the PDFs wherever you keep them.

## Setup

1. Install Ollama and pull a model: `ollama pull qwen2.5:3b`.
2. In Gmail, create the label you want, for example "Invoices".
3. Import `workflow.json` into n8n.
4. Connect a Gmail OAuth2 credential on **Watch Gmail Inbox** and **Add Invoice Label**.
5. Open **Add Invoice Label** and pick your label under Label Names or IDs.
6. If n8n runs in Docker, open **Set Label Settings** and change `ollama_url` to `http://host.docker.internal:11434`.
7. Activate the workflow.

## What it does not do

- It does not read amounts, due dates or line items, and it does not open PDFs.
- It does not save attachments anywhere. They are passed along for a node you add.
- It does not go back through old email. Only emails that arrive after you activate it are checked.
- It does not mark, move or archive anything. It only adds a label.
- It works with Gmail only.

## Tested

Six sample emails were run through a test copy on n8n 2.35.7 with `qwen2.5:3b`. See [proof/README.md](proof/README.md).

Get more free templates by email: https://stats.sender.net/forms/dR630O/view

Paid templates: https://airankings-templates.whop.site/?ref=gh-gmail-invoice-labels-ollama
