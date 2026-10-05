# Vezna for Claude

![Vezna](assets/logo.png)

[Vezna](https://vezna.ai) is an AI-native accounting firm for Bulgarian companies. Its agents keep the books from your bank feeds and invoices, matching every payment to its document and asking when something is missing, and accountants review the work and file the returns.

This plugin connects Claude to your company's books in Vezna and teaches it three workflows:

- **Month-end check** (`month-end-check`): what is still missing before the month can close and the VAT return can be filed, with the estimated VAT position and its deadline.
- **Find missing invoices** (`find-missing-invoices`): search the company mailbox for the supplier invoices Vezna is waiting for and submit them, so Vezna files them and matches them to their payments.
- **Explain the numbers** (`explain-the-numbers`): a short management summary of a month's revenue, costs, operating result and cash, and what changed against the month before.

You can also just ask, for example:

- "What's our cash position across all accounts, and how has it changed in the last 30 days?"
- "What is Vezna still waiting on from us this month?"
- "How did we do last month compared with the month before?"

## What it connects to

The plugin adds one remote MCP server, `https://app.vezna.ai/mcp`, run by Ninefold Ltd (Vezna). It runs no local code, has no hooks and installs no packages. The first time Claude uses it, you sign in to Vezna, choose the company and approve the connection (OAuth 2.1 with PKCE). Each connection covers one company.

Through that server Claude can read cash, P&L, spending, revenue, the VAT estimate, the company profile and the general ledger (trial balance, accrual P&L, open balances, the draft VAT return, journal entries), list the documents Vezna is waiting for, and search the company mailbox when one is connected to Vezna. Two tools add to the books: `submit_document` (files an invoice or receipt) and `answer_question` (records your answer to one of Vezna's questions).

The plugin can't make payments, file returns, approve or edit journal entries, change settings or delete data. VAT figures are estimates and ledger figures are drafts until your accountant reviews them.

## Requirements

A Vezna account as an owner or team member of the company (accountants can't connect an assistant to a client's books). Searching and filing from email also needs the company's Gmail mailbox to be connected to Vezna.

## Privacy and data

Your books stay in Vezna. Tool calls go only to `app.vezna.ai`; the plugin sends nothing anywhere else. Personal identification numbers (ЕГН/ЛНЧ) and full card numbers are masked in every result, every tool call is logged in Vezna, and you can revoke the connection at any time on Vezna's Connections page or by disconnecting it in Claude.

- Privacy policy: https://vezna.ai/privacy/
- Terms: https://vezna.ai/terms/
- Setup and troubleshooting: https://vezna.ai/docs/connect/
- Support: support@vezna.ai (https://vezna.ai/support/)

## License

MIT. See [LICENSE](LICENSE).
