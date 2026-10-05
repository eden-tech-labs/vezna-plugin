---
name: month-end-check
description: Month-end check with Vezna — what is still missing before the month can close and the VAT return can be filed, plus the estimated VAT position and its deadline. Use when the user asks "are we ready for month-end", "what's missing before VAT", "what do I still need to send", or wants a closing checklist for a month.
license: MIT
---

# Month-end check: what's missing before VAT

Builds a short, actionable checklist for one month from the company's books in Vezna. Every figure must come from a Vezna tool result in this conversation.

## Steps

1. **Open items.** Call `list_open_items` — with `month` (`YYYY-MM`) if the user named one; without it, the tool picks the month currently being closed and says which month it used. Note the documented-coverage line ("Documented: X of Y") and every item waiting on the client, with its id, what is needed and how long it has been open.
2. **Use that month throughout** and say which month it is.
3. **VAT estimate.** Call `vat_position` with the same month for the estimated output VAT, input credit, net payable or refundable, and the filing deadline.
4. **Ledger readiness.** Call `ledger_status` to see whether that month's journal entries are still proposals or already posted by the accountant, and whether the month is closed.
5. **Report**, in this order:
   - one line: month, coverage (X of Y documented), estimated VAT (pay or reclaim, EUR) and the filing deadline;
   - **For you to do**: the client-side items grouped by kind — invoices or receipts to send, sales invoices to issue, questions to answer — each with the amount, the date and the age;
   - **With the accountant**: items already answered or on the accountant's side (count only, unless the user asks);
   - **Ledger**: proposed vs posted for the month, and whether it is closed.
6. **Offer the next step.** For missing supplier invoices, offer to look for them in the company mailbox (see the `find-missing-invoices` skill). For questions, ask the user for the answer; don't guess.

## Rules

- The VAT figure is an **estimate**; the accountant prepares and files the return. Never say the return is filed or final.
- Don't call `submit_document` or `answer_question` from this skill without the user's explicit go-ahead for that specific item.
- If `list_open_items` says nothing is waiting, say so plainly and still report the VAT estimate and ledger status.
- Keep it short: a checklist, not an essay. Use EUR and name the month.
- Don't give tax or legal rulings (for example on deadlines beyond what the tools return, or penalties); point the user to their accountant.

## Example

User: "Are we ready to close September?"

Expected tool calls: `list_open_items(month="2026-09")`, `vat_position(month="2026-09")`, `ledger_status()`.

Answer shape: "September: 41 of 44 payments documented. Estimated VAT: pay €2,850, due 14 October. For you to do: 2 supplier invoices to send (…), 1 payment to explain (…). With the accountant: 3 items. Ledger: 38 entries proposed, 0 posted; September is open. Want me to look for the two invoices in the company mailbox?"
