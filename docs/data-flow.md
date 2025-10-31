# Data Flow

## Initial User Onboarding Flow

### 1. Landing Page
- User visits Keyv.ai
- Clicks "Sign In with Google"

### 2. OAuth Flow
- Frontend redirects to API OAuth endpoint
- API initiates Google OAuth 2.0 flow
- User grants Gmail read permissions
- Google returns authorization code
- API exchanges code for access token
- Session created and stored in Redis

### 3. Post-OAuth Setup
- User redirected to Dashboard
- API triggers initial background task via `/tasks` endpoint
- Background Worker queued for first email scan

### 4. First Email Scan
- Background Worker connects to Gmail API
- Scans emails from past 1 year (default)
- Passes raw emails to Context Engine
- Context Engine classifies and extracts payment data
- Payment data stored in PostgreSQL
- Dashboard now shows upcoming payments

## Ongoing Payment Tracking Flow

### 1. Periodic Email Scanning (Every 6 hours)
- Background Worker runs scheduled task
- For each connected user:
  - Fetches new emails since last scan
  - Respects user's configured time range
  - Uses Goroutines for parallel processing
- Raw emails sent to Context Engine
- Context Engine processes:
  - Classifies payment vs non-payment
  - Extracts payment details if relevant
  - Discards non-payment emails
- Extracted payment data sent to API
- API updates PostgreSQL with new/updated payments
- Redis cache invalidated for affected users

### 2. Dashboard Refresh
- User opens Dashboard
- Frontend calls API `/payments` endpoint
- API checks Redis cache first
- If cache miss, queries PostgreSQL
- Returns upcoming payments sorted by due date
- Frontend displays payment list with status

### 3. Settings Update
- User changes time range in Settings page
- Frontend calls API `/user/settings` endpoint
- API updates user preferences in PostgreSQL
- Next background scan uses new time range
- Historical data outside new range remains but not updated

## Authentication Flow

1. User visits site
2. Clicks "Sign In with Google"
3. Redirected to Google OAuth consent screen
4. User grants Gmail read permissions
5. Google redirects back with auth code
6. API exchanges code for access & refresh tokens
7. Tokens stored securely (encrypted in Redis)
8. Session token returned to frontend
9. Frontend stores session token
10. All subsequent API calls include session token
11. API validates token on each request

## Email Processing Pipeline

```
User Gmail → Background Worker → Context Engine → API → PostgreSQL
                                                    ↓
                                                  Redis
                                                    ↓
                                                Frontend
```

### Step-by-Step

1. **Email Retrieval**
   - Background Worker fetches emails via Gmail API
   - Only new emails since last scan
   - Respects user's time range setting

2. **Classification**
   - Raw email sent to Context Engine
   - ML model classifies: payment or non-payment
   - Non-payment emails discarded immediately

3. **Extraction**
   - Payment emails processed for information
   - Extracts: payee, amount, due date, status
   - Structured data created

4. **Storage**
   - Extracted data sent to API
   - API validates and stores in PostgreSQL
   - Redis cache updated

5. **Display**
   - User opens Dashboard
   - Frontend fetches from API
   - Upcoming payments displayed
