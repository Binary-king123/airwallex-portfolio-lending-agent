## 💡 The Problem

Revenue-based lending platforms must balance two competing goals: funding approved borrowers on time and preserving enough liquidity to meet future obligations.

A borrower may qualify for an advance, but releasing the entire amount could push the platform below its required reserve. Unexpectedly low repayments or delayed revenue make this decision even harder.

**LiquidityGuard AI** addresses this challenge by continuously evaluating available liquidity, borrower repayment activity, and portfolio constraints before deciding whether to fund, partially fund, defer, or reject an advance.

## 🚀 Our Solution

The agent follows a risk-aware lending workflow built around Airwallex Sandbox APIs.

- **Observe:** Retrieve account balances and relevant borrower repayment information.
- **Assess:** Calculate available liquidity after preserving the mandatory reserve.
- **Decide:** Determine whether the advance can be funded in full, partially funded, or deferred.
- **Execute:** Collect eligible repayments and disburse approved advances using the appropriate sandbox APIs.
- **Reconcile:** Verify transaction outcomes and reassess lending capacity when new revenue or repayment information arrives.

## 🧠 What Makes It Different?

Rather than treating lending as a one-time approval, LiquidityGuard AI treats it as a continuously changing portfolio decision.

A delayed deposit, lower-than-expected repayment, or competing funding request can change the safest allocation. The agent must respond to that change instead of blindly following its original decision.

The design separates financial policy from API execution: deterministic code enforces liquidity limits, while the orchestration layer coordinates observations, decisions, actions, and reconciliation.

## 🛡️ Safety by Design

- Enforce a configurable portfolio reserve floor.
- Use precise decimal arithmetic for monetary calculations.
- Prevent duplicate transfers through verified idempotency mechanisms.
- Revalidate liquidity before executing funding decisions.
- Fail safely when required financial data is unavailable.
- Maintain an auditable record of decisions and transaction outcomes.

## 🏗️ Technology

Java 21 · Spring Boot 3 · REST APIs · Airwallex Sandbox · Maven · JUnit 5

## 🎯 Expected Impact

LiquidityGuard AI aims to help lending platforms make faster, more consistent funding decisions while reducing avoidable liquidity risk and manual intervention.

**The goal is simple: fund responsibly, adapt to changing cash flows, and protect the portfolio's ability to meet its obligations.**

*Built for the Agentic Banking Hackathon — Starter Kit 07: Portfolio Lending Agent. All transactions are intended for sandbox testing.*

### Project Repository

https://github.com/Binary-king123/airwallex-portfolio-lending-agent
