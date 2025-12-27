# Kiwis.club Documentation

## Overview

Kiwis.club is an intelligent payment tracking system that securely extracts upcoming payment information from Gmail without storing or reading email content. The system uses a microservices architecture with Next.js frontend, Golang API, PostgreSQL database, Redis caching, background task workers, and an AI-powered context engine for payment classification and extraction.

## Documentation Structure

- [System Architecture](./system-architecture.md) - High-level system design and component overview
- [Components](./components.md) - Detailed component specifications
- [Data Flow](./data-flow.md) - User flows and data processing pipelines
- [Infrastructure](./infrastructure.md) - Deployment, scaling, and monitoring
- [Security & Privacy](./security-privacy.md) - Security architecture and privacy design
- [Technology Stack](./technology-stack.md) - Technologies and frameworks used

## Quick Links

### For Developers
- [Components](./components.md) - Understand each service component
- [Data Flow](./data-flow.md) - Learn how data moves through the system
- [Technology Stack](./technology-stack.md) - See what technologies we use

### For DevOps
- [Infrastructure](./infrastructure.md) - Deployment and scaling strategies
- [Security & Privacy](./security-privacy.md) - Security implementation details

### For Product/Business
- [System Architecture](./system-architecture.md) - High-level system overview
- [Security & Privacy](./security-privacy.md) - Privacy-first design principles

## Key Features

- **Privacy-First**: Emails never stored, processed in-memory only
- **Intelligent Extraction**: AI-powered payment classification and extraction
- **Secure**: Google OAuth 2.0, encrypted tokens, minimal permissions
- **Scalable**: Microservices architecture with horizontal scaling
- **User Control**: Configurable time ranges and data management

## Getting Started

1. Read [System Architecture](./system-architecture.md) for overview
2. Review [Components](./components.md) to understand each service
3. Check [Technology Stack](./technology-stack.md) for implementation details
4. Follow [Infrastructure](./infrastructure.md) for deployment
