# **Pandoc** — Healthcare Management System  
_MERN + Payments + **GPT-4/RAG** assistant (streaming)_

[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?logo=typescript&logoColor=white)](#)
[![React](https://img.shields.io/badge/React-18.x-61DAFB?logo=react&logoColor=0d1117)](#)
[![Node.js](https://img.shields.io/badge/Node.js-20.x-339933?logo=node.js&logoColor=white)](#)
[![MongoDB](https://img.shields.io/badge/MongoDB-6.x-47A248?logo=mongodb&logoColor=white)](#)
[![Stripe|PayPal](https://img.shields.io/badge/Payments-Stripe%20%7C%20PayPal-6c3bf5)](#)
[![RAG](https://img.shields.io/badge/AI-RAG%20%2B%20GPT--4-8A2BE2)](#)
[![License](https://img.shields.io/badge/License-MIT%20or%20Private-999999)](#)

> ⚕️ **Non-diagnostic assistant.** The chatbot provides general guidance and helps users find the right doctors on the platform. It **does not** diagnose, prescribe, or book on a user’s behalf.

---

## 🔥 Why Pandoc?

Modern clinics need **one** system that does it all without duct-tape:

- **Web**: polished patient UX, reliable doctor/admin dashboards, rock-solid payments.  
- **AI**: a **RAG-grounded** assistant that cites platform policies and routes users to the right specialty—**fast, safe, and streaming**.

Pandoc blends both: **production-grade MERN** + **safety-first AI integration**.

---

## 📌 Table of Contents

1. [Features](#-features)  
2. [Architecture](#-architecture)  
3. [Tech Stack](#-tech-stack)  
4. [Monorepo Layout](#-monorepo-layout)  
5. [Quick Start](#-quick-start)  
6. [Configuration](#-configuration)  
7. [Running with Docker](#-running-with-docker)  
8. [API Surface (Web)](#-api-surface-web)  
9. [AI Integration (RAG + GPT-4)](#-ai-integration-rag--gpt-4)  
10. [Security & Compliance](#-security--compliance)  
11. [Testing & Quality](#-testing--quality)  
12. [Observability & Ops](#-observability--ops)  
13. [Roadmap](#-roadmap)  
14. [License](#-license)

---

## 🚀 Features

### Patient
- Doctor directory (specialty, location, languages, rating)
- Real-time **slot availability**
- Checkout & receipts (Stripe/PayPal)
- Profile, upcoming visits, reschedule/cancel (policy-aware)
- **Chat assistant**: policy answers, doctor recommendations, **prefilled booking links**

### Doctor
- Calendar (day/week), availability editor, OOO blocks  
- Appointment queue, patient notes  
- Basic analytics (no-show rate, utilization)

### Admin
- Doctors, specialties, schedules, blackout rules  
- Payments, refunds, ledger & audit logs  
- Policies (cancellation/refund/telehealth) — versioned  
- CSV exports & reports

### AI (built for product, not demos)
- **RAG over platform docs** (policies, profiles, specialties)  
- Tools: `search_doctors`, `peek_slots`, `create_prefilled_booking_link`  
- **Streaming** answers with cited policy snippets  
- **Guardrails**: non-diagnostic, red-flag escalation (ER/urgent care), **PII redaction**  
- Budget caps & caching to control token spend

---

## 🧭 Architecture

flowchart LR
  subgraph Web_Clients
    A[Patient App (React)]
    D[Doctor/Admin Apps (React)]
  end

  A -->|REST/JWT| B[API - Node/Express]
  D -->|REST/JWT| B

  B --> E[(MongoDB)]
  B --> F[Payments (Stripe/PayPal Webhooks)]

  subgraph AI_Service
    H[AI Orchestrator]
    G[(Vector Store)]
    I[(Embeddings)]
    L[GPT-4 (Tool Calls)]
  end

  A <-->|SSE Streaming| H
  H --> G
  H --> I
  H --> L
  H --> B
