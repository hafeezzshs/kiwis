# Components

## Frontend Layer (Next.js - Port 3000)

### Landing Page
- Public-facing entry point
- Product information
- "Sign In with Google" button
- Marketing content

### Dashboard
- Displays upcoming payments only
- Shows payment status (pending/paid)
- Refreshes on page load (syncs with 6h email scan cycle)
- Clean, minimal interface focused on next payments due

### Settings Page
- User profile details
- Gmail connection status indicator
- Time range configuration for email scanning:
  - Past day
  - Past week
  - Past month
  - Past quarter (3 months)
  - Past 6 months
  - Past year (default)
- OAuth connection management

## Backend Layer (Golang API - Port 8000)

### OAuth Endpoints
- Google OAuth 2.0 integration
- User authentication flow
- Gmail API permission requests:
  - Read-only access to emails
  - Metadata access only
- Token management and refresh
- Session creation

### User Endpoints
- Fetch user profile details
- Update user settings
- Get Gmail connection status
- Configure email scan time range
- Manage user preferences

### Payments Endpoints
- Fetch upcoming payments
- Get payment status (pending/paid)
- Retrieve payment history
- Filter by date range
- Sort by due date

### Tasks Endpoint
- Trigger background email scan
- Initiate context engine processing
- Queue email processing jobs
- Monitor task status

## Background Task Worker (Golang)

Asynchronous email processing system using Goroutines.

### Core Responsibilities

**1. Periodic Email Scanning (Every 6 hours)**
- Connects to Gmail API for each user
- Scans for new emails within configured time range
- Uses concurrent Goroutines for parallel processing
- Fetches raw email data without storing content
- Passes raw emails directly to Context Engine

**2. Email Processing Pipeline**
- Retrieves emails based on user's time range setting
- Filters by date range (default: past 1 year)
- Processes emails in batches
- Maintains scan state to avoid reprocessing
- Handles API rate limits and retries

**3. Task Queue Management**
- Manages email processing jobs
- Handles failed processing attempts
- Monitors queue health
- Logs processing metrics

**4. Future Enhancements**
- Gmail webhook integration (push notifications)
- Real-time email processing
- Reduced polling frequency
- Instant payment updates

## Context Engine (AI/ML Classification & Extraction)

### Privacy-First Processing
- Receives raw email data directly from Background Worker
- No email content stored in PayPulse Pro systems
- Processes emails in-memory only
- Extracts payment information only

### Classification Pipeline

**Step 1: Payment Email Detection**
- Classifies if email is payment-related
- Identifies bills, invoices, subscriptions, payment reminders
- Filters out non-payment emails immediately
- Uses ML models trained on payment patterns

**Step 2: Information Extraction**
- Extracts only payment-relevant data:
  - Payee (who to pay)
  - Payer (user/account)
  - Amount
  - Due date
  - Payment status (pending/paid)
  - Payment method (if available)
  - Reference/Invoice number
- Ignores business-sensitive information
- Anonymizes unnecessary details

**Step 3: Data Storage**
- Sends extracted payment data to API
- API stores structured payment info in PostgreSQL
- Original email content never persisted
- Only payment metadata retained

### Technology
- NLP models for text extraction
- Pattern matching for common payment formats
- Entity recognition for amounts, dates, names
- Confidence scoring for accuracy

## Data Layer

### PostgreSQL (Primary Database)

**Tables**:
- `users`: User accounts, Gmail connection status, preferences
- `payments`: Extracted payment information (payee, amount, due date, status)
- `scan_history`: Email scan logs and timestamps
- `user_settings`: Time range preferences, notification settings

**Key Fields in Payments Table**:
- Payment ID
- User ID
- Payee name
- Amount
- Currency
- Due date
- Status (pending/paid)
- Extracted date
- Last updated
- Source (Gmail message ID reference only)

### Redis (Cache & Session Store)
- User session tokens
- OAuth access tokens (encrypted)
- Gmail API rate limit tracking
- Cached upcoming payments for quick dashboard loads
- Background task queue status
- TTL-based cache invalidation (6h aligned with scan cycle)
