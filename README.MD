# FinTrackr

FinTrackr is a personal finance tracking web application designed to help users manage bills, track income, forecast cash flow, and receive AI-powered financial insights.

This project starts as a personal financial management tool but is designed with the potential to evolve into a SaaS platform.

---

## Purpose

FinTrackr focuses on real-world financial behavior:

- Tracking recurring bills and income
- Forecasting upcoming expenses and balances
- Identing financial risk periods
- Providing AI-driven budgeting insights
- Helping users avoid late fees and cash flow surprises

Unlike many budgeting apps, FinTrackr prioritizes bill timing and income flow rather than just spending categories.

---

## Core Features

### Bill Management

- Recurring bills (rent, utilities, subscriptions, insurance)
- One-time expenses
- Due date tracking
- Payment history logging
- Upcoming bill alerts

### Income Tracking

- Salary, freelance, reimbursements
- Recurring or irregular income
- Timeline view of expected deposits

### Cash Flow Forecasting

- Monthly projected balances
- Upcoming income vs bills
- Overdraft risk detection
- Expense trend analysis

### AI Insights (Optional)

Examples:

- Spending pattern analysis
- Budget health summaries
- Risk predictions
- Suggestions like:
  - shifting bill timing
  - subscription reduction opportunities
  - savings recommendations

Natural language queries:

"Can I afford a $300 payment next month?"
"What bills usually cause problems?"
"Where is most of my money going?"

---

## Recommended Tech Stack

### Backend

- ASP.NET Core Web API (.NET 8)
- Entity Framework Core
- PostgreSQL or Azure SQL Database

### Frontend

- Angular or React
- JWT authentication

### Hosting (Simple Deployment)

- Azure App Service (backend API)
- Azure Static Web Apps or Vercel (frontend)
- Azure SQL/Postgres database

This keeps infrastructure straightforward for an MVP.

---

## API Keys & Secrets Management

This application may use API keys for:

- AI services (OpenAI / Azure OpenAI)
- Financial data aggregation (Plaid, Finicity)
- Notification services (email/SMS)
- Database credentials

### Best Practices

#### Never commit secrets to Git

- No API keys in source code
- No secrets in public repositories

#### Use environment variables

Local development:

.env file (not committed):

OPENAI_API_KEY=your_key_here
DB_CONNECTION_STRING=your_connection_string
JWT_SECRET=your_secret_key

#### Production (Azure Recommended)

Use:

- Azure Key Vault for secrets storage
- Azure App Service environment variables
- Managed identities where possible

This ensures:

- Secure credential rotation
- Centralized secret management
- No exposure in logs or repos

---

## Authentication Plan

- JWT-based user authentication
- Password hashing with bcrypt or similar
- Optional future integration:
  - OAuth login (Google, Microsoft)
  - Multi-factor authentication

---

## Database Concept (High-Level)

### Users

Account details and preferences.

### Bills

Recurring obligations and payment history.

### Income

Income sources and individual events.

### Accounts (Optional Future)

Checking, savings, credit cards.

### Insights

AI-generated recommendations and financial flags.

---

## MVP Development Order

1. User authentication
2. Bills CRUD
3. Income tracking
4. Cash flow forecasting
5. Dashboard UI
6. Basic AI insights

---

## Future Enhancements

### Finance Integrations

- Bank syncing via Plaid
- Automatic transaction import

### Analytics

- Spending categorization
- Predictive budgeting
- Net worth tracking

### SaaS Features

- Multi-tenant support
- Subscription billing
- Admin dashboards

### Mobile

- Companion mobile app
- Push notifications

---

## Security Considerations

- Encrypt sensitive data
- Use HTTPS everywhere
- Store secrets securely
- Never store bank credentials directly
- Follow financial data best practices

---

## License

TBD (MIT, proprietary, or commercial SaaS model).

---

## Vision

FinTrackr aims to provide:

- Clarity about finances
- Predictive financial awareness
- AI-assisted budgeting
- A calm, actionable financial dashboard

Built to solve real problems first — scale later.
