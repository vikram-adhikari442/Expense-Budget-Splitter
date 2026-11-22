# Expense-Budget-Splitter
# Expense & Budget Splitter

A command-line expense tracking, bill-splitting, and monthly budget management engine written in **C++17** and backed by **SQLite3**. Built and tested inside **GitHub Codespaces**.

---

## Core Capabilities

- **Exact Currency Handling:** Stores all INR values as 64-bit integer **paise** (`std::int64_t`)—eliminating IEEE-754 floating-point drift during splits and settlements.
- **Equal & Unequal Splits:** Validates that member shares sum exactly to the total expense (`isValidSplit()`) before committing any rows.
- **Monthly Category Budgets:** Tracks category spend against monthly limits (`budgets`) and flags over-budget transactions.
- **ACID Persistence:** Uses SQLite transactions (`BEGIN` / `COMMIT` / `ROLLBACK`), `PRAGMA foreign_keys = ON`, and parameterized `sqlite3_prepare_v2` queries.

---

## Architecture & Schema

```text
members (id, name, created_at)
   │
   ├──< expenses (id, payer_id, budget_id, title, total_paise, spent_date)
   │       │
   │       └──< expense_splits (expense_id, member_id, share_paise)
   │
budgets (id, category, monthly_limit_paise, month_year)
