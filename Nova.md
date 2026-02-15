# Nova: Financial Navigation System

> Money is not about restriction.  
> Money is about direction.

Nova is your AI-powered financial navigation system that clarifies trajectory, reduces friction, and protects your future self.

**Not judgment. Not hype. Not noise. Just trajectory.**

---

## Table of Contents

- [Core Capabilities](#core-capabilities)
- [Personality Modes](#personality-modes)
- [System Architecture](#system-architecture)
- [Tool API](#tool-api)
- [Training & Implementation](#training--implementation)
- [Security & Guardrails](#security--guardrails)
- [Implementation Roadmap](#implementation-roadmap)
- [Core Philosophy](#core-philosophy)

---

## Core Capabilities

### 1. Cash Flow Forecasting

Nova projects your financial future with precision:

- **Project balance forward in time** - See where you'll be weeks or months ahead
- **Detect overdraft risk** - Get early warnings before problems occur
- **Identify bill clustering** - Understand when multiple bills hit at once
- **Simulate purchase scenarios** - "What if I buy this?" before you commit
- **Show safe-to-spend amount** - Know exactly how much you can safely spend

**Tool Examples:**

```python
get_cashflow_forecast(start_date, end_date)
simulate_purchase(amount, date)
list_upcoming_bills(days)
```

### 2. Goal Intelligence

Nova tracks and optimizes your savings goals with mathematical precision.

**Each goal includes:**
- Target amount
- Deadline
- Required monthly contribution
- Actual contribution
- Gap analysis
- Probability of success

**Nova can:**
- Calculate required savings velocity
- Compare actual vs required pace
- Recommend reallocation from discretionary categories
- Offer micro-commitment actions

**Example:**

> "Goal probability currently 38%. Redirecting 40% of Amazon spending restores trajectory."

**Tool Examples:**

```python
get_goal_status(goal_id)
simulate_reallocation(amount, category, goal)
```

### 3. Behavioral Pattern Detection

Nova identifies spending patterns and anomalies to help you understand your financial behavior.

**Nova detects:**
- Recurring subscription patterns
- Spending spikes
- Merchant anomalies
- Paycheck irregularities
- Lifestyle drift

**Nova references:**
- Rolling averages
- Frequency changes
- Z-score anomaly detection
- Merchant clustering

**Example:**

> "Amazon spend exceeded 90-day average by 62%. Pattern similar to last post-payday cycle."

**Tools:**

```python
get_behavior_summary(period)
detect_anomalies(range)
```

### 4. Encouragement Engine

Nova provides actionable encouragement, never vague motivation.

**Encouragement structure:**
1. State current reality (with numbers)
2. Compare to required pace
3. Identify behavioral lever
4. Offer specific action

**Correct:**

> "You contributed $100. Required pace is $800. You're $700 behind. Redirecting 50% of discretionary flow fixes this."

**Incorrect:**

> "You got this!"

---

## Personality Modes

Nova adapts to your communication style while maintaining mathematical accuracy.

### Navigator Mode (Default)
Balanced encouragement + precision

### Analyst Mode
Direct, concise, data-focused

### Boost Mode
Higher warmth, identity reinforcement

> **Important:** Mode affects tone only — not data or logic.

---

## System Architecture

Nova is implemented as an AI orchestration layer on top of FinTrackr's deterministic finance engine.

### Key Principle

**The finance engine computes facts.**  
**Nova (LLM) turns facts into plans, explanations, and next actions.**

Nova does not "wing it" with numbers.

### System Components

#### 1. Data Ingestion Layer

**Purpose:** Connect accounts, import transactions, normalize raw data.

- Bank connections handled through OAuth-based aggregator
- FinTrackr never stores bank credentials
- Raw provider payloads stored for audit/debugging (optional)
- App uses normalized internal schema

**Ingestion jobs:**
- Sync accounts and balances
- Sync transactions (posted + pending)
- Detect recurring transactions and merchant patterns
- Backfill historical data on first connection

#### 2. Normalization Layer (Canonical Ledger)

**Purpose:** Convert messy financial data into a stable internal model.

**Normalized entities:**
- `Account` - Bank accounts and balances
- `Transaction` - Individual financial transactions
- `Merchant` - Normalized name + aliases
- `RecurringPattern` - Expected cadence + amount range
- `Bill` - Explicit scheduled obligations
- `IncomeEvent` - Expected deposits
- `Goal` - Target amount + deadline + pace

This layer enables forecasting and makes Nova's answers consistent.

#### 3. Finance Engine (Deterministic Core)

**Purpose:** Compute truth.

**This engine owns:**
- Cash flow forecasts
- "Safe to spend" calculations
- Bill clustering detection
- Goal pace math
- Anomaly detection metrics
- Scenario simulations

**Critical:** This logic lives outside the LLM. The LLM requests computations through tools.

#### 4. Nova Orchestration Layer (LLM + Tools)

**Purpose:** Interpret intent and call tools, then produce a response.

**Flow:**
```
User message
→ Intent + entities extraction
→ Tool calls (finance engine)
→ Response composition (tone + plan + next actions)
```

**Nova only speaks using:**
- Tool results (numbers)
- User preferences (tone/mode)
- Goal settings
- System rules (guardrails)

#### 5. UI Layer

**Purpose:** Show outputs in ways that build trust.

**Examples:**
- Forecast timeline / calendar overlay of bills
- Goal trajectory bar (required pace vs actual)
- "Why this alert" explanation cards
- Suggested actions (create goal, set transfer, adjust bill estimate)

---

## Tool API

Nova uses a "toolbox API" - internal backend endpoints or functions exposed to the AI runtime.

### Forecasting

```python
get_cashflow_forecast(user_id, start_date, end_date, scenario_id?)
get_safe_to_spend(user_id, as_of_date)
```

### Bills / Income

```python
list_upcoming_bills(user_id, days)
list_expected_income(user_id, days)
create_bill(user_id, name, amount_rule, recurrence_rule, due_rule)
```

### Goals

```python
create_goal(user_id, name, target_amount, deadline, priority)
get_goal_status(user_id, goal_id)
recommend_goal_contribution(user_id, goal_id)
```

### Anomalies / Patterns

```python
detect_anomalies(user_id, start_date, end_date, sensitivity)
summarize_spending(user_id, period, group_by)
detect_recurring_merchants(user_id)
```

### Simulations

```python
simulate_purchase(user_id, amount, date, merchant_or_category)
simulate_reallocation(user_id, from_category, amount, to_goal)
```

### Tool Response Format

All tool calls must return:
- Exact numeric values
- Short explanation string ("how computed")
- Metadata for the UI (confidence, thresholds, reasons)

---

## Training & Implementation

### How Nova Is "Trained"

**Nova is not trained on your personal banking data.**

**Default approach:**
- Use a general-purpose LLM (pretrained)
- Constrain it with:
  - System prompt rules (behavior + safety)
  - Tool calling (numbers come from the finance engine)
  - Retrieval (optional) for your own notes and goal context

This avoids leaking personal data into model training and keeps outputs accurate.

### Optional: Retrieval (RAG)

If you add Retrieval:

**Store embeddings for:**
- Merchant aliases ("AMZN MKTP" = Amazon)
- User notes ("this charge is reimbursable")
- Goal narratives ("Norway trip plan")
- Previous explanations ("why January is tight")

Nova retrieves relevant context snippets, then still calls tools for the math.

### Optional: Fine-tuning (Not Required)

If fine-tuning is added later:

**Fine-tune only on:**
- Synthetic conversations
- Labeled intents
- "Good response structure" examples
- **Not** raw transaction history

**Goal:** Better intent routing and consistent phrasing, not better math.

---

## Security & Guardrails

### Rules

- **Nova must never invent balances, totals, or dates**
- If a tool response is missing data, Nova asks to connect accounts or define bills/goals
- Nova must cite its reasoning path: *"Based on forecast from Feb 14–May 14 and your Norway goal pace…"*

### Permissions

- Tools run only within the authenticated user scope
- No cross-user data access
- Sensitive tokens encrypted at rest
- Strict separation from LLM prompt logs

### Actions

- Nova can propose actions (set goal, create bill, simulate plan)
- Any "money-moving" action (if ever added) requires explicit confirmation and an approval flow

### Data Protection

- Never stores bank credentials
- Operates within authenticated user scope
- Cannot move money without explicit approval layer
- Does not provide financial legal/tax advice

Nova explains calculations when asked.

---

## Implementation Roadmap

### MVP Order

1. **Goals + manual bills + forecast engine**
   - Core functionality without bank connections

2. **Nova tool calling on top of forecast + goals**
   - AI layer becomes useful immediately

3. **Bank sync for transactions**
   - Real data integration

4. **Recurring detection + anomaly detection**
   - Advanced pattern recognition

This keeps Nova useful even before bank connections are perfect.

---

## Behavioral Metrics (Future Expansion)

Nova may compute system-generated metrics to anchor behavioral insight:

- **Savings Velocity Score** - How fast you're moving toward goals
- **Impulse Index** - Spending pattern consistency
- **Goal Alignment Rating** - How well spending matches goals
- **Cash Flow Stability Score** - Predictability of income/expenses
- **Subscription Drag Ratio** - Impact of recurring charges

---

## Core Philosophy

> Money is not about restriction.  
> Money is about direction.

### Nova Exists To:

- **Clarify trajectory** - Show where you're headed
- **Reduce friction** - Make financial decisions easier
- **Reinforce identity** - Support who you want to become
- **Prevent drift** - Keep you on track
- **Protect future you** - Make decisions that benefit tomorrow

**Nova is your financial navigation system.**

Not judgment.  
Not hype.  
Not noise.

**Just trajectory.**
