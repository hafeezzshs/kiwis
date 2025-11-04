# Security & Privacy

## Privacy-First Design

### Email Content Protection
- Raw emails never stored in Kiwis.club systems
- Emails processed in-memory only by Context Engine
- Only payment metadata extracted and stored
- No access to email body content after processing
- Gmail API used with minimal read-only permissions

### Data Minimization
- Only payment-relevant information extracted
- Business-sensitive details ignored
- Personal information anonymized where possible
- User can delete all data anytime

## Authentication & Authorization

### Google OAuth 2.0
- Industry-standard OAuth flow
- Minimal Gmail permissions requested
- Token refresh handling
- Secure token storage (encrypted in Redis)
- Session expiration and renewal

### API Security
- Session token validation on every request
- Rate limiting per user
- Input validation and sanitization
- SQL injection prevention
- CORS configuration

## Data Security

### Encryption
- TLS/SSL for all data in transit
- Encrypted OAuth tokens at rest
- Database encryption at rest
- Secure credential management

### Access Control
- User can only access their own data
- API enforces user-level isolation
- Background Worker processes user data separately
- Context Engine stateless processing

## Compliance Considerations

- GDPR compliance (data minimization, right to deletion)
- User consent for Gmail access
- Transparent data usage
- Privacy policy and terms of service
- Audit logging for data access

## Security Best Practices

### API Layer
- Rate limiting to prevent abuse
- Input validation on all endpoints
- Parameterized queries to prevent SQL injection
- HTTPS only (no HTTP)
- Secure headers (HSTS, CSP, etc.)

### Data Layer
- Encrypted connections to database
- Regular security patches
- Backup encryption
- Access logs and monitoring

### Application Layer
- Dependency vulnerability scanning
- Regular security audits
- Penetration testing
- Incident response plan

## Privacy Guarantees

### What We Store
- User email address (from Google OAuth)
- Payment metadata only:
  - Payee name
  - Amount
  - Due date
  - Status
  - Gmail message ID (reference only)

### What We Never Store
- Full email content
- Email body text
- Email attachments
- Sender/recipient details (except payee)
- Email subject lines
- Any non-payment related information

### User Rights
- View all stored data
- Delete account and all data
- Revoke Gmail access anytime
- Export payment data
- Control time range settings
