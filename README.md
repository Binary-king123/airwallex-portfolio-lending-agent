# airwallex-portfolio-lending-agent
Autonomous Portfolio Lending Agent (Starter Kit 07) built with Java 21 and Spring Boot leveraging Airwallex Sandbox APIs.
markdown
# 🤖 Autonomous Portfolio Lending Agent (Starter Kit 07)

[![Java Version](https://shields.io)](https://openjdk.org)
[![Spring Boot](https://shields.io)](https://spring.io)
[![Platform](https://shields.io)](https://airwallex.com)
[![Hackathon Track](https://shields.io)](#)

An enterprise-grade, event-driven banking engine built with **Java 21** and **Spring Boot 3.x**. This intelligent agent automates dynamic debt-collection pipelines and evaluates incoming credit issuance requests completely hands-free by interfacing directly with **Airwallex Sandbox REST APIs**.

---

## 🎯 Core Engineering Objectives

The agent strictly implements the risk guardrails defined in **Starter Kit 07 (Portfolio Lending Agent)**:
* **Liquidity Floor Defense:** Dynamically parses account holdings to protect a strict **\$10,000,000.00 USD reserve floor** from being breached by outstanding dynamic loans.
* **Autonomous Micro-Sizing:** If an active loan request threatens the reserve limit, the engine automatically downsizes the allocation payload to the maximum allowable safe headroom rather than flat-out failing.
* **Idempotency Guarantees:** Applies cryptographic, unique tracing IDs across all mutable payloads (`POST /api/v1/transfers/create`) to prevent dual-allocation over-processing during network dropouts.

---

## 🏗️ Technical Architecture & Design Patterns

Use code with caution.
[ Incoming Dynamic Loan Request ]
│
▼
┌────────────────────────────────────────────────────────┐
│ Auth Loop: Fetch/Renew 30-Min JWT Token via /login     │
└────────────────────────────────────────────────────────┘
│
▼
┌────────────────────────────────────────────────────────┐
│ Ledger Audit: Pull Live USD Liquidity via /balances     │
└────────────────────────────────────────────────────────┘
│
▼
Is (USD Balance - Loan Request) >= $10M?
│
┌───────────────┴───────────────┐
│ YES                           │ NO
▼                               ▼
┌───────────────────────┐       ┌───────────────────────┐
│   DECISION: APPROVE   │       │   DECISION: MITIGATE  │
└───────────────────────┘       └───────────────────────┘
│                               │
▼                               ▼
┌───────────────────────┐       ┌───────────────────────┐
│ 1. Create Beneficiary │       │ Recalculate Safe Size │
│ 2. Execute Transfer   │       │ Or Reject Transaction │
└───────────────────────┘       └───────────────────────┘

### Key Engineering Subsystems
1. **`LendingAgentService`**: Coordinates token lifecycles, parses JSON payload data directly into localized memory maps, and handles exception routing.
2. **Deterministic Token Daemon**: A background service that checks token age and seamlessly refreshes the short-lived session token every 25 minutes to ensure uninterrupted 24/7 autonomous monitoring.
3. **Liquidity Analysis Guard**: A thread-safe checking method that blocks outgoing race-conditions where two parallel loan applications might run simultaneously.

---

## 💻 Code Blueprints & Implementation

### 1. Externalized Application Configurations
The system is built to safely segregate credentials using a Spring validation structure inside `src/main/resources/application.properties`:

```properties
# Airwallex Sandbox API Gateway Credentials
airwallex.client-id=ArayTeSJRsGNp5OkRHY5...
airwallex.api-key=your_secret_api_key_here
airwallex.base-url=https://mcp.sandbox.airwallex.com/developer

# Starter Kit 07 Core Business Constraints
lending.reserve-floor=10000000.00

2. Thread-Safe Risk Assessment Engine

java
package com.example.hackathon.service;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;

@Service
public class RiskAssessmentEngine {

    @Value("\${lending.reserve-floor}")
    private double reserveFloor;

    /**
     * Evaluates lending eligibility deterministically against vault baselines.
     * Amounts are calculated strictly in major decimal units (Not cents).
     */
    public synchronized LoanDecision evaluateLiquidityRisk(double requestedAmount, double totalWalletBalance) {
        double maxLendableFunds = totalWalletBalance - reserveFloor;

        if (maxLendableFunds <= 0) {
            return new LoanDecision(false, 0.0, "Vault balance is already at or below the \$10M reserve floor limit.");
        }

        if (requestedAmount <= maxLendableFunds) {
            return new LoanDecision(true, requestedAmount, "Approved: Full amount within safe liquidity headroom.");
        } else {
            // Adaptive Downsizing Fallback Pattern
            return new LoanDecision(true, maxLendableFunds, "Modified Approval: Loan downsized to protect reserve floor.");
        }
    }
}
Use code with caution.

⚡ Quick Start & Deployment Guide


Prerequisites

• Java 21 SDK (OpenJDK / Corretto)
• Maven 3.9+ or embedded wrapper
• Active Airwallex Developer Sandbox Account

1. Build and Compile Workspace

bash
git clone https://github.com/Binary-king123/airwallex-portfolio-lending-agent.git
cd airwallex-portfolio-lending-agent
./mvnw clean compile
Use code with caution.

2. Run the Application

bash
./mvnw spring-boot:run
Use code with caution.
The system will initialize, hook into the Airwallex secure gateway sandbox loop, and immediately output its token status along with wallet analytics directly to the console.
