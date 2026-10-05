---
name: explain-the-numbers
description: Explain a month's numbers from the company's books in Vezna — revenue, costs, operating result and cash — and what changed against the previous month, with the drivers behind the change. Use when the user asks "how did we do this month", "why are costs up", "explain our numbers", "what changed since last month" or wants a short management summary.
license: MIT
---

# Explain this month's numbers

Turns Vezna's figures into a short management summary: what happened, what changed and why. Every figure must come from a Vezna tool result in this conversation; never estimate or fill gaps.

## Steps

1. **Pick the month.** Use the month the user names; otherwise the last complete month. Compare it with the month before.
2. **Headline.** Call `pl_summary` for the year (both years if the two months span a year boundary). Take revenue, operating costs and the operating result for both months.
3. **Drivers.** Call `category_breakdown` for each of the two months (`month_from` = `month_to` = the month) and find the categories that moved most. For the top one or two, call `search_transactions` with that `category`, the month range and `group_by="counterparty"` to name who is behind the change.
4. **Revenue mix**, if revenue moved noticeably: call `revenue_streams` for the year.
5. **Cash.** Call `cash_position` for current cash and its 30-day change.
6. **Report:**
   - one line with revenue, costs and the operating result for the month in EUR, and the change against the previous month;
   - two or three drivers, each with the amount and the counterparty behind it;
   - current cash and its 30-day change;
   - one caveat line: these are cash-basis management figures (money in and out of the bank), the latest days may not be synced yet, and tax, dividends, loans and internal transfers are excluded from the operating result.

## Accrual view

If the user asks for profit "by invoice date", unpaid invoices or accrual figures, use `profit_and_loss_accrual` and `open_items`, and call `ledger_status` first so you can say whether the figures are posted by the accountant or still proposals.

## Rules

- Name the period and say EUR. Don't mix tax, dividends, loans or internal transfers into operating costs.
- No forecasts and no tax or legal advice; point the user to their accountant for those.
- If a tool says the month predates the data window, say so instead of approximating.
- Keep it tight: lead with the answer, then the drivers.

## Example

User: "Why did our costs go up in August?"

Expected tool calls: `pl_summary(year=2026)`, `category_breakdown(month_from="2026-07", month_to="2026-07")`, `category_breakdown(month_from="2026-08", month_to="2026-08")`, `search_transactions(category="contractor", month_from="2026-08", month_to="2026-08", group_by="counterparty")`.
