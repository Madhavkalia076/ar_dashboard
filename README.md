# Accounts Receivable Dashboard (MySQL + Python/Flask)

A minimal AR dashboard with:
- MySQL schema + seed data
- Flask APIs (parameterized)
- Aging bucket utility + unit tests
- Single-page UI with filters, KPIs, invoice table, and a Top-5 chart

## 1) Setup MySQL

```sql
SOURCE schema.sql;
SOURCE seed.sql;
```

> Ensure your MySQL user/password can access the `ar_dashboard` database.

## 2) Configure and run backend (Python 3.10+)

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# Linux/Mac:
source .venv/bin/activate

pip install -r requirements.txt

# Optionally export DB credentials
# PowerShell
$env:DB_HOST="localhost"; $env:DB_USER="root"; $env:DB_PASSWORD="yourpassword"; $env:DB_NAME="ar_dashboard"
# bash
export DB_HOST=localhost DB_USER=root DB_PASSWORD=yourpassword DB_NAME=ar_dashboard

python app.py
```

The app runs on http://localhost:5000

## 3) UI
Open `http://localhost:5000/` in your browser.
- Filters: Customer + Invoice date range
- KPIs: Total Invoiced, Received, Outstanding, % Overdue
- Table: sortable, client-side search, overdue rows highlighted
- Action: **Record Payment** (partial allowed)
- Chart: Top 5 customers by total outstanding

## 4) API Endpoints

- `GET /customers` → list customers
- `GET /invoices?customer_id=&start_date=YYYY-MM-DD&end_date=YYYY-MM-DD`
- `GET /kpis?customer_id=&start_date=&end_date=`
- `GET /top5`
- `POST /payments` → JSON `{ "invoice_id": 1, "payment_date": "2025-08-20", "amount": 1000.0 }`

All queries are parameterized.

## 5) Tests

```bash
python -m unittest tests/test_aging.py
```

## Notes
- Aging bucket is computed only on **unpaid portion** and only if `due_date < today`.
- Overpayment checks are basic; extend as needed.
