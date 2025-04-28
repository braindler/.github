# Braindler 🚀 — AI-Native Conversational Platform

![Project Status](https://img.shields.io/badge/Status-Active-brightgreen)  
![Technology](https://img.shields.io/badge/Stack-LLM%20%7C%20RAG%20%7C%20n8n%20Automation%20%7C%20FastAPI-orange)

---

## Overview

**Braindler** is a next-generation AI-native conversational platform, evolving from the historical Braindler-Legacy pre-AI bots.  
It combines the power of **Large Language Models (LLMs)**, **Retrieval-Augmented Generation (RAG)**, and **Business automation** to create intelligent, scalable assistants for real-world business use.

Braindler enables companies to deliver human-like dialogues, personalized interactions, and deep integration with external services — all through modular, open-source architecture.

---
## What is Braindler-Assistant?

Braindler-Assistant представляет собой мультиканального AI-помощника, ориентированного на поддержку и продажи через мессенджеры.

## 🧠 Что такое Braindler-Assistant?

**Braindler-Assistant** — это интеллектуальный бот, работающий в мессенджерах (Telegram, WhatsApp, Line, WeChat, Instagram), который:

- Следует заранее заданному **AI-чат-скрипту** — блочной структуре с описанием сообщений и возможными ответами.
- Если пользователь выходит за рамки **AI-чат-скрипта**, бот пытается **импровизировать**
- Предлагаются пожелания для улучшения **AI-чат-скрипта** на основе пользовательских диалогов.
- При неоюходимости параллельно пересылает сообщения оператору, позволяя ему вмешиваться в диалог в реальном времени.
- Если оператор не отвечает в течение определенного времени, ии-автоматически продолжает диалог.
- Запоминает уникальный стиль ответов разных операторов и старается не смешивать их между собой.
- Поддерживает мультиязычное общение, корректируя ответы для улучшения пользовательского опыта

---

## 🚀 Основные возможности

- **Мультиканальная поддержка**: Telegram, WhatsApp, Line, WeChat, Instagram.
- **AI-чат-скрипты**: гибкая настройка диалогов с возможностью импровизации.
- **Полуавтоматический режим**: оператор может вмешиваться в диалог в реальном времени.
- **Обучение на опыте**: бот предлагает обновления скриптов на основе предыдущих диалогов.
- **Уникальные стили операторов**: бот запоминает и различает стили общения разных операторов.
- **Мультиязычность**: поддержка общения на разных языках с автоматической коррекцией. ([Braindler: Internet-Enabled AI for Real-World Tasks, Goal ... - GitHub](https://github.com/braindler/Braindler?utm_source=chatgpt.com))

---

## 🧩 Применение

- **Поддержка клиентов**: автоматизация ответов на часто задаваемые вопросы.
- **Продажи**: ведение диалогов с потенциальными клиентами и сопровождение сделок.
- **Маркетинг**: проведение опросов, рассылок и акций через мессенджеры по клиентской базе с согласия пользователя.
- **Обратная связь**: сбор отзывов и предложений от пользователей.

---

## 🔧 Технологии

-
- **Интеграции**: Telegram API, WhatsApp Business API, Line Messaging API и другие.


Braindler-Assistant — это мощный инструмент для автоматизации общения с клиентами, сочетающий в себе гибкость AI и контроль оператора.


## Why Braindler?

- 🧠 RAG-based knowledge retrieval ensures accurate and grounded answers.
- 🌐 Multichannel support: Telegram bots, Web chat, Slack, and more.
- 🔥 Native multilingual communication — business speaks in Thai, client in Russian, English, Chinese.
- ⚙️ Deep business process automation using **n8n** workflows.
- 📚 Persistent memory per user for contextual continuity.
- 📈 Scalable microservice architecture with full monitoring and analytics.
- 🛡️ Open-source and self-hostable, with full data control.

---

## System Architecture

Braindler is modular and scalable by design.  
**Main components:**

- **User Interface (e.g., Telegram Bot)**  
  Thin client handling user interactions. Easily extendable to Web, Slack, WhatsApp, etc.

- **Backend Server (Core Application)**  
  Orchestrates message handling, RAG retrieval, LLM generation, and integrations via FastAPI.

- **RAG Service (Knowledge Retrieval)**  
  Vector database (e.g., FAISS, Milvus) + Embedding models for semantic search and augmentation.

- **LLM Module**  
  Locally deployed LLM (e.g., Llama 3.1) serving through an RPC or HTTP API.

- **Integration Modules**  
  Seamless business actions via n8n workflows: CRM updates, calendar events, email dispatch, etc.

- **Databases and Storages**  
  - PostgreSQL for user data, dialogues, integrations.
  - Vector storage for knowledge base.
  - Logs and telemetry metrics for monitoring.

---

### 📈 Data Flow

> **User → Bot → Backend → RAG Retrieval → LLM Generation → Action Handling (n8n) → Bot → User**

All interactions are logged and monitored, with rich metrics and telemetry.

---

## Monitoring and Alerting

Braindler includes a complete observability stack:

- **Prometheus + Grafana**: Monitoring response times, resource usage, and service health.
- **Alertmanager**: Automatic alerts on service failures, latency spikes, or resource exhaustion.
- **Sentry**: Error tracking across backend and bot layers.

Key monitored metrics include:

| Area | Metrics | Alerts |
|:----|:-------|:------|
| Performance | Request time, RAG/LLM latency | Response time > 3 sec |
| Resources | GPU %, CPU %, RAM usage | High CPU/RAM/Disk alerts |
| Stability | Service uptime, error rates | 5xx errors, container restarts |
| Quality | Answer success rates, RAG recall | Low RAG precision, empty answers |

---

## KPIs for RAG Module

| KPI | Target (v1.0) |
|:----|:-------------|
| Precision@3 | > 0.8 |
| Recall | > 0.9 |
| RAG Usage Rate | > 70% |
| User Satisfaction | > 4/5 |
| RAG Latency | < 500 ms |

---

## Technology Stack

| Layer | Technology |
|:------|:-----------|
| Frontend | Telegram API, Web Chat (future) |
| Backend | FastAPI (Python 3.11) |
| Knowledge Base | FAISS / Milvus |
| LLM | Local deployment (Llama 3.1) |
| Automation | n8n (self-hosted) |
| Databases | PostgreSQL, Redis |
| Monitoring | Prometheus, Grafana, Sentry |
| Deployment | Docker, Kubernetes-ready |

---

## Future Roadmap

- 🌐 Web client interface (SPA)
- 🗣️ Multi-turn memory improvements (semantic compression)
- 🤖 Intelligent fallback from LLM to scripted flows
- 🔐 Enterprise data privacy modes
- ✨ Visual flow editor for sales funnels and automations
- 🧩 Plugin architecture for third-party extensions
- 📚 Continual knowledge base enrichment with automatic indexing

---

## Credits

Developed by [Anton Dodonov](https://github.com/AntonDodonov),  
Founder of [NativeMindNet](https://github.com/NativeMindNet) and [Braindler](https://github.com/braindler).

---

> 🧠 *Braindler is where AI meets real-world business automation.*
