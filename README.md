# airwallex-portfolio-lending-agent
Autonomous Portfolio Lending Agent (Starter Kit 07) built with Java 21 and Spring Boot leveraging Airwallex Sandbox APIs.
# Autonomous Portfolio Lending Agent (Starter Kit 07)

An enterprise-grade, event-driven banking agent built with **Java 21** and **Spring Boot 3.x** designed for the **Agentic Banking Hackathon**. This agent programmatically interfaces with the **Airwallex Sandbox REST APIs** to manage credit allocation risk dynamically while guarding a firm liquidity limit.

## 🚀 Key Features
* **Automated Risk Guardrails:** Evaluates and sizes all dynamic lending proposals instantly against a strict **\$10,000,000 USD reserve floor**.
* **Idempotent Outbound Settlements:** Utilizes unique tracking IDs across all mutation payloads (`POST /api/v1/transfers/create`) to ensure network resilience.
* **Resilient Session Lifecycles:** Implements a managed background daemon that intercepts, tracks, and refreshes short-lived API Bearer tokens every 25 minutes.

## 🛠️ Tech Stack
* **Language/Runtime:** Java 21 LTS
* **Framework:** Spring Boot 3.4+ / Spring Web
* **API Integration:** Airwallex Developer Platform (REST Services & Sandbox Node Simulators)
* **Configuration & Environment Management:** Spring Boot Properties System

## 📋 System Evaluation Pipeline (Starter Kit 07 Core Logic)
