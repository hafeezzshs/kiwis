# System Architecture

## Overview

PayPulse Pro is an intelligent payment tracking system that securely extracts upcoming payment information from Gmail without storing or reading email content. The system uses a microservices architecture with Next.js frontend, Golang API, PostgreSQL database, Redis caching, background task workers, and an AI-powered context engine for payment classification and extraction.

## High-Level Architecture

```
┌─────────────┐
│  Landing    │
│    Page     │
└──────┬──────┘
       │
       ▼ (Google OAuth)
┌─────────────┐         ┌─────────────┐
│  Dashboard  │────────▶│  Settings   │
│   (Next.js) │         │    Page     │
└──────┬──────┘         └──────┬──────┘
       │                       │
       │ (Fetch Payments)      │ (Connect Gmail)
       │                       │
       ▼                       ▼
┌──────────────────────────────────────┐
│         API (Golang)                 │
│  ┌────────────┬──────────┬────────┐ │
│  │   OAuth    │ Payments │ Tasks  │ │
│  │ Endpoints  │Endpoints │Endpoint│ │
│  └────────────┴──────────┴────────┘ │
└───────┬──────────────────────┬───────┘
        │                      │
        ▼                      ▼
   ┌─────────┐         ┌──────────────┐
   │  Redis  │         │  PostgreSQL  │
   │ (Cache) │         │  (Payments)  │
   └─────────┘         └──────────────┘
                               ▲
                               │
                               │ (Store Payment Data)
                               │
        ┌──────────────────────┴─────────────┐
        │                                    │
        ▼                                    │
┌────────────────┐                           │
│  Background    │                           │
│  Task Worker   │                           │
│  (Golang)      │                           │
│                │                           │
│  - Goroutines  │                           │
│  - Periodic    │                           │
│    Email Scan  │                           │
│    (Every 6h)  │                           │
└────────┬───────┘                           │
         │                                   │
         │ (Raw Email Data)                  │
         │                                   │
         ▼                                   │
┌────────────────┐                           │
│ Context Engine │                           │
│  (AI/ML)       │                           │
│                │                           │
│ 1. Classify    │───────────────────────────┘
│ 2. Extract     │    (Payment Info Only)
│                │
│ - Payment/Not  │
│ - Payee/Payer  │
│ - Amount/Date  │
│ - Status       │
└────────────────┘
```

## Service Communication Flow

```
Frontend (3000)
    │
    ├─── OAuth ──────────────────┐
    ├─── Fetch Payments ─────────┤
    ├─── User Settings ──────────┤
    │                            │
    ▼                            ▼
                          API (8000)
                               │
                ┌──────────────┼──────────────┐
                │              │              │
                ▼              ▼              ▼
            Redis         PostgreSQL    Gmail API
            (Cache)       (Storage)     (Read-only)
                                             │
                                             │
                                             ▼
                                    Background Worker
                                      (Goroutines)
                                             │
                                             │ (Raw Email)
                                             ▼
                                    Context Engine
                                    (Classify & Extract)
                                             │
                                             │ (Payment Data)
                                             ▼
                                    API (Store in DB)
```

## Core Components

### 1. Frontend (Next.js - Port 3000)
User-facing web application with landing page, dashboard, and settings.

### 2. API (Golang - Port 8000)
Core backend service handling OAuth, payments, user management, and task coordination.

### 3. Background Worker (Golang)
Asynchronous email scanning service using Goroutines for parallel processing.

### 4. Context Engine (AI/ML)
Privacy-first payment classification and extraction service.

### 5. PostgreSQL (Port 5432)
Primary database storing users, payments, and scan history.

### 6. Redis (Port 6379)
Cache and session store for performance optimization.

## Key Design Principles

### Privacy-First
- Emails never stored in PayPulse Pro systems
- In-memory processing only
- Minimal Gmail permissions (read-only)
- User data deletion on request

### Scalability
- Microservices architecture
- Horizontal scaling for API and workers
- Goroutine-based concurrency
- Redis for distributed caching

### Security
- Google OAuth 2.0 authentication
- Encrypted tokens at rest
- TLS/SSL for all communications
- User-level data isolation

### Performance
- 6-hour scan intervals
- Incremental email processing
- Redis caching layer
- Batch processing

## Port Allocation

| Service           | Port | Type     | Technology |
| ----------------- | ---- | -------- | ---------- |
| Frontend          | 3000 | Web App  | Next.js    |
| API               | 8000 | Backend  | Golang     |
| PostgreSQL        | 5432 | Database | PostgreSQL |
| Redis             | 6379 | Cache    | Redis      |
| Background Worker | N/A  | Worker   | Golang     |
| Context Engine    | TBD  | Service  | AI/ML      |
