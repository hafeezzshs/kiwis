# Technology Stack

## Frontend

- **Framework**: Next.js (React)
- **Language**: TypeScript/JavaScript
- **Styling**: TailwindCSS (or similar)
- **State Management**: React Context/Redux
- **HTTP Client**: Axios/Fetch API
- **Port**: 3000

## Backend API

- **Language**: Golang
- **Framework**: Gin/Echo/Chi (Go web framework)
- **Authentication**: OAuth 2.0 (Google)
- **Gmail Integration**: Google Gmail API SDK
- **Port**: 8000

## Background Worker

- **Language**: Golang
- **Concurrency**: Goroutines
- **Scheduler**: Cron-like scheduling
- **Gmail API**: Google API Client
- **Port**: N/A (background process)

## Context Engine

- **Language**: Python (for ML) or Golang
- **ML Framework**: TensorFlow/PyTorch or spaCy
- **NLP**: Natural Language Processing libraries
- **Pattern Matching**: Regex + ML models
- **Port**: TBD

## Databases

### PostgreSQL
- **Version**: 15+
- **Purpose**: Primary data store
- **Port**: 5432
- **Tables**:
  - users
  - payments
  - scan_history
  - user_settings

### Redis
- **Version**: 7+
- **Purpose**: Cache and session store
- **Port**: 6379
- **Use Cases**:
  - Session tokens
  - OAuth tokens (encrypted)
  - Payment cache
  - Rate limiting
  - Task queue status

## Infrastructure

- **Containerization**: Docker
- **Orchestration**: Docker Compose (dev) / Kubernetes (prod)
- **Reverse Proxy**: Nginx
- **Load Balancer**: Nginx/HAProxy
- **CI/CD**: GitHub Actions / GitLab CI
- **Monitoring**: Prometheus + Grafana
- **Logging**: ELK Stack / Loki

## Development Tools

- **Version Control**: Git
- **Code Quality**: golangci-lint, ESLint, Prettier
- **Testing**: Go testing, Jest, React Testing Library
- **API Documentation**: Swagger/OpenAPI
- **Database Migrations**: golang-migrate / Flyway

## Cloud Services (Production)

### AWS
- **Compute**: ECS/EKS for containers
- **Database**: RDS for PostgreSQL
- **Cache**: ElastiCache for Redis
- **Storage**: S3 for static assets
- **CDN**: CloudFront
- **Monitoring**: CloudWatch

### Google Cloud Platform
- **Compute**: Cloud Run / GKE
- **Database**: Cloud SQL for PostgreSQL
- **Cache**: Memorystore for Redis
- **Storage**: Cloud Storage
- **CDN**: Cloud CDN
- **Monitoring**: Cloud Monitoring

## External APIs

- **Google OAuth 2.0**: User authentication
- **Gmail API**: Email access (read-only)
- **Context Engine**: Payment classification (internal/external)

## Security Tools

- **SSL/TLS**: Let's Encrypt / AWS Certificate Manager
- **Secrets Management**: HashiCorp Vault / AWS Secrets Manager
- **Vulnerability Scanning**: Snyk / Trivy
- **WAF**: AWS WAF / Cloudflare
