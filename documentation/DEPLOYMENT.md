# Deployment Guide

## Overview

{{DEPLOYMENT_OVERVIEW}}

**Deployment Platform:** `{{PLATFORM}}` (AWS / GCP / Azure / Kubernetes / Docker / etc.)

**CI/CD Tool:** `{{CI_CD_TOOL}}` (GitHub Actions / GitLab CI / CircleCI / Jenkins)

**Infrastructure as Code:** `{{IAC_TOOL}}` (Terraform / Pulumi / CloudFormation)

## Architecture

### Environments

```
Development (dev)
├── Database: {{DEV_DB}}
├── API: {{DEV_API_URL}}
└── Frontend: {{DEV_FRONTEND_URL}}

Staging (stage)
├── Database: {{STAGE_DB}}
├── API: {{STAGE_API_URL}}
└── Frontend: {{STAGE_FRONTEND_URL}}

Production (prod)
├── Database: {{PROD_DB}}
├── API: {{PROD_API_URL}}
└── Frontend: {{PROD_FRONTEND_URL}}
```

### Infrastructure Components

```
{{INFRASTRUCTURE_DIAGRAM}}
┌─────────────┐
│   Users     │
└──────┬──────┘
       │
┌──────▼──────┐
│  CloudFront │ CDN
│  / Load     │
│  Balancer   │
└──────┬──────┘
       │
┌──────▼──────────────────┐
│   Application Servers   │
│  ┌─────┐  ┌─────┐      │
│  │App 1│  │App 2│      │
│  └─────┘  └─────┘      │
└──────┬──────────────────┘
       │
┌──────▼──────┐
│   Database  │
│  (Primary + │
│   Replica)  │
└─────────────┘
```

## Prerequisites

### Required Tools

```bash
# Install required CLI tools
{{INSTALL_CLI_TOOLS}}

# Examples:
# - AWS CLI: https://aws.amazon.com/cli/
# - Docker: https://www.docker.com/
# - Kubernetes: https://kubernetes.io/
# - Terraform: https://www.terraform.io/
```

### Required Permissions

**Cloud Provider:**
- `{{CLOUD_PERMISSIONS}}`

**CI/CD:**
- `{{CI_CD_PERMISSIONS}}`

### Required Secrets

Set these secrets in your CI/CD platform:

```bash
# Cloud Provider
AWS_ACCESS_KEY_ID=...
AWS_SECRET_ACCESS_KEY=...

# Database
DATABASE_URL=...
DATABASE_PASSWORD=...

# API Keys
API_KEY=...
SECRET_KEY=...

# OAuth
OAUTH_CLIENT_ID=...
OAUTH_CLIENT_SECRET=...
```

## Infrastructure Setup

### Using Terraform

```bash
# Initialize Terraform
cd infrastructure/terraform
terraform init

# Plan infrastructure changes
terraform plan -var-file=environments/dev.tfvars

# Apply changes
terraform apply -var-file=environments/dev.tfvars

# Destroy infrastructure (use with caution)
terraform destroy -var-file=environments/dev.tfvars
```

### Infrastructure Modules

```
infrastructure/
├── terraform/
│   ├── modules/
│   │   ├── vpc/
│   │   ├── database/
│   │   ├── compute/
│   │   └── storage/
│   ├── environments/
│   │   ├── dev/
│   │   ├── stage/
│   │   └── prod/
│   └── main.tf
└── scripts/
    ├── setup.sh
    └── teardown.sh
```

### Key Resources

**Networking:**
- VPC / Virtual Network
- Subnets (public/private)
- Security Groups / Firewall Rules
- Load Balancer

**Compute:**
- EC2 instances / VMs
- ECS / EKS / Kubernetes
- Lambda / Functions

**Database:**
- RDS / Cloud SQL
- ElastiCache / Redis
- S3 / Storage

**CDN & DNS:**
- CloudFront / Cloud CDN
- Route53 / CloudDNS

## Containerization

### Dockerfile

```dockerfile
# {{DOCKERFILE_EXAMPLE}}
# Stage 1: Build
FROM {{BUILD_IMAGE}} AS builder
WORKDIR /app
COPY package*.json ./
RUN {{INSTALL_COMMAND}}
COPY . .
RUN {{BUILD_COMMAND}}

# Stage 2: Production
FROM {{PRODUCTION_IMAGE}}
WORKDIR /app
COPY --from=builder /app/{{BUILD_OUTPUT}} ./app
EXPOSE {{PORT}}
CMD ["{{START_COMMAND}}"]
```

### Docker Compose (Development)

```yaml
# docker-compose.yml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "{{PORT}}:{{PORT}}"
    environment:
      - NODE_ENV=development
      - DATABASE_URL=postgres://user:pass@db:5432/mydb
    depends_on:
      - db
      - redis
    volumes:
      - .:/app
      - /app/node_modules

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7
    ports:
      - "6379:6379"

volumes:
  postgres_data:
```

### Building Containers

```bash
# Build image
docker build -t {{IMAGE_NAME}}:{{TAG}} .

# Tag for registry
docker tag {{IMAGE_NAME}}:{{TAG}} {{REGISTRY}}/{{IMAGE_NAME}}:{{TAG}}

# Push to registry
docker push {{REGISTRY}}/{{IMAGE_NAME}}:{{TAG}}
```

## CI/CD Pipeline

### Pipeline Overview

```
{{CI_CD_PIPELINE}}
┌───────┐   ┌───────┐   ┌───────┐   ┌───────┐   ┌───────┐
│  Code │──>│ Lint  │──>│ Build │──>│ Test  │──>│Deploy │
│ Push  │   │       │   │       │   │       │   │       │
└───────┘   └───────┘   └───────┘   └───────┘   └───────┘
                 │                       │
                 └────── Fail ───────────┘
```

### GitHub Actions Example

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Lint
        run: {{LINT_COMMAND}}

  test:
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - uses: actions/checkout@v3
      - name: Run tests
        run: {{TEST_COMMAND}}

  build:
    runs-on: ubuntu-latest
    needs: test
    if: github.event_name == 'push'
    steps:
      - uses: actions/checkout@v3
      - name: Build Docker image
        run: docker build -t ${{ env.IMAGE_NAME }}:${{ github.sha }} .
      - name: Push to registry
        run: |
          docker login -u ${{ secrets.REGISTRY_USER }} -p ${{ secrets.REGISTRY_PASSWORD }}
          docker push ${{ env.IMAGE_NAME }}:${{ github.sha }}

  deploy:
    runs-on: ubuntu-latest
    needs: build
    if: github.ref == 'refs/heads/main'
    steps:
      - name: Deploy to production
        run: {{DEPLOY_COMMAND}}
```

### GitLab CI Example

```yaml
# .gitlab-ci.yml
stages:
  - lint
  - test
  - build
  - deploy

lint:
  stage: lint
  script: {{LINT_COMMAND}}

test:
  stage: test
  script:
    - {{TEST_COMMAND}}
  coverage: '/Coverage: \d+%/'

build:
  stage: build
  script:
    - docker build -t $IMAGE_NAME:$CI_COMMIT_SHA .
    - docker push $IMAGE_NAME:$CI_COMMIT_SHA
  only:
    - main

deploy_production:
  stage: deploy
  script:
    - {{DEPLOY_COMMAND}}
  environment:
    name: production
    url: {{PROD_URL}}
  only:
    - main
  when: manual
```

### Deployment Strategies

#### Blue-Green Deployment

```
Current (Blue)      New (Green)
┌─────────┐        ┌─────────┐
│ App v1  │        │ App v2  │
│  100%   │   →    │   0%    │
└─────────┘        └─────────┘
     │                  │
     └──────Traffic─────┘

After verification:
Current (Blue)      New (Green)
┌─────────┐        ┌─────────┐
│ App v1  │        │ App v2  │
│   0%    │        │  100%   │
└─────────┘        └─────────┘
```

#### Canary Deployment

```
┌─────────┐
│ App v1  │  90% traffic
└─────────┘
     │
┌────▼────┐
│ App v2  │  10% traffic (canary)
└─────────┘

Gradually shift:
┌─────────┐
│ App v1  │  0%
└─────────┘
     │
┌────▼────┐
│ App v2  │  100%
└─────────┘
```

#### Rolling Deployment

```
Instance 1: [v1] → [v2] → [v2]
Instance 2: [v1] → [v1] → [v2]
Instance 3: [v1] → [v1] → [v2]

Step 1: Deploy to instance 1
Step 2: Deploy to instance 2
Step 3: Deploy to instance 3
```

## Environment Variables

### Environment Configuration

```bash
# .env.development
NODE_ENV=development
DATABASE_URL=postgresql://localhost:5432/mydb_dev
API_URL=http://localhost:3000
LOG_LEVEL=debug

# .env.production
NODE_ENV=production
DATABASE_URL=${PROD_DATABASE_URL}
API_URL=https://api.myapp.com
LOG_LEVEL=info
```

### Managing Secrets

**Do NOT commit secrets to version control.**

```bash
# Use environment variables
export DATABASE_URL="postgres://..."

# Use secret managers
{{SECRET_MANAGER_USAGE_EXAMPLE}}

# Use encrypted secrets
{{ENCRYPTED_SECRETS_EXAMPLE}}
```

## Database Management

### Production Database

**Setup:**
- Enable automated backups
- Configure read replicas
- Set up failover
- Enable SSL
- Configure connection pooling

**Migrations:**
```bash
# Production migrations
{{PROD_MIGRATE_COMMAND}}

# Best practices:
# - Always backup before migrating
# - Run migrations during low traffic
# - Test migrations in staging first
```

**Backups:**
```bash
# Create backup
{{BACKUP_COMMAND}}

# Restore from backup
{{RESTORE_COMMAND}}

# Automated backups
- Daily full backups
- Point-in-time recovery enabled
- Backup retention: 30 days
```

## Monitoring & Observability

### Infrastructure Monitoring

**Tool:** `{{MONITORING_TOOL}}` (Datadog / New Relic / Prometheus + Grafana)

**Key Metrics:**
- CPU utilization
- Memory usage
- Disk I/O
- Network throughput
- Request latency
- Error rate
- Request throughput

### Application Monitoring

**APM Tool:** `{{APM_TOOL}}`

**Track:**
- Response time (p50, p95, p99)
- Error rates
- Database query performance
- External API calls
- Custom metrics

### Log Aggregation

**Tool:** `{{LOG_TOOL}}` (ELK Stack / Splunk / CloudWatch Logs)

**Log Levels:**
- ERROR: Critical failures
- WARN: Potential issues
- INFO: Business events
- DEBUG: Debugging information

### Alerting

**Critical Alerts:**
- Application down
- Error rate > 5%
- Response time p95 > 2s
- CPU > 90%
- Memory > 90%
- Disk > 85%

**Warning Alerts:**
- Error rate > 1%
- Response time p95 > 1s
- CPU > 70%
- Memory > 70%

### Health Checks

```{{LANGUAGE}}
// Health check endpoint
app.get('/health', (req, res) => {
  const health = {
    uptime: process.uptime(),
    message: 'OK',
    timestamp: Date.now(),
    checks: {
      database: await checkDatabase(),
      redis: await checkRedis(),
      externalApi: await checkExternalApi()
    }
  };
  
  res.status(200).json(health);
});

// Readiness check
app.get('/ready', (req, res) => {
  // Check if app is ready to receive traffic
  const isReady = await checkReadiness();
  res.status(isReady ? 200 : 503).json({ ready: isReady });
});
```

## Scaling

### Horizontal Scaling

```bash
# Increase replicas
kubectl scale deployment {{APP_NAME}} --replicas=5

# Auto-scaling configuration
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: {{APP_NAME}}-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: {{APP_NAME}}
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

### Vertical Scaling

```bash
# Increase instance size
- Small → Medium: 2x memory/CPU
- Medium → Large: 2x memory/CPU
```

### Database Scaling

**Read Replicas:**
```sql
-- Route read queries to replica
-- Write queries go to primary
PRIMARY DATABASE (Read/Write)
    │
    └─── REPLICA 1 (Read-only)
    └─── REPLICA 2 (Read-only)
```

**Connection Pooling:**
```bash
# Use PgBouncer for PostgreSQL
pgbouncer --ini /etc/pgbouncer/pgbouncer.ini
```

## Security

### Network Security

**VPC Configuration:**
- Private subnets for databases
- Public subnets for load balancers
- Security groups restricting access
- VPN/bastion host for admin access

**SSL/TLS:**
- Enforce HTTPS for all traffic
- Use valid SSL certificates (Let's Encrypt)
- Enable HSTS headers
- Use TLS 1.2+

### Access Control

**IAM Roles:**
- Least privilege principle
- Separate roles for different services
- Regular permission audits

**Secret Management:**
```
AWS Secrets Manager / Parameter Store
HashiCorp Vault
Azure Key Vault
GCP Secret Manager
```

### Security Checklist

- [ ] Enable SSL/TLS for all connections
- [ ] Use IAM roles instead of access keys
- [ ] Enable audit logging
- [ ] Configure WAF (Web Application Firewall)
- [ ] Set up DDoS protection
- [ ] Enable encryption at rest
- [ ] Regularly rotate secrets
- [ ] Configure rate limiting
- [ ] Set up CSP headers
- [ ] Enable VPN/bastion access

## Rollback Strategy

### Application Rollback

```bash
# Kubernetes rollback
kubectl rollout undo deployment/{{APP_NAME}}

# Docker rollback
kubectl set image deployment/{{APP_NAME}} {{APP_NAME}}={{IMAGE_NAME}}:previous-tag

# Versioned deployment rollback
# Simply deploy previous version tag
```

### Database Rollback

```bash
# Always have migration rollback ready
# Each migration should have "down" migration

# Rollback last migration
{{MIGRATE_ROLLBACK_COMMAND}}

# Emergency: Restore from backup
{{RESTORE_FROM_BACKUP_COMMAND}}
```

### Rollback Procedure

1. **Identify Issue:**
   - Monitor alerts trigger
   - Error rates spike
   - Performance degrades

2. **Decision:**
   - Assess severity
   - Estimate fix time
   - Decide: rollback or forward fix

3. **Execute Rollback:**
   ```bash
   # Notify team
   # Rollback application
   kubectl rollout undo deployment/{{APP_NAME}}
   
   # Verify rollback successful
   kubectl rollout status deployment/{{APP_NAME}}
   
   # Monitor metrics
   ```

4. **Post-Rollback:**
   - Document incident
   - Fix issue in development
   - Test fix thoroughly
   - Deploy fix

## Post-Deployment

### Verification Steps

```bash
# 1. Check application health
curl https://api.myapp.com/health

# 2. Check database connectivity
curl https://api.myapp.com/health/database

# 3. Check logs for errors
kubectl logs -f deployment/{{APP_NAME}} | grep ERROR

# 4. Monitor metrics
- Response time
- Error rate
- CPU/Memory usage

# 5. Run smoke tests
npm run test:smoke
```

### Deployment Checklist

**Pre-Deployment:**
- [ ] All tests passing
- [ ] Code reviewed and approved
- [ ] Changelog updated
- [ ] Database migrations tested
- [ ] Rollback plan documented
- [ ] On-call engineer notified

**During Deployment:**
- [ ] Monitor deployment logs
- [ ] Check application health
- [ ] Verify database migrations
- [ ] Run integration tests

**Post-Deployment:**
- [ ] Verify all endpoints working
- [ ] Check error rates
- [ ] Monitor performance metrics
- [ ] Update documentation if needed
- [ ] Notify team of completion

## Troubleshooting

### Common Issues

**Pod Crashes:**
```bash
# Check logs
kubectl logs deployment/{{APP_NAME}}

# Describe pod for events
kubectl describe pod {{POD_NAME}}

# Check resource limits
kubectl top pods
```

**Database Connection Issues:**
```bash
# Check connection pool
# Verify DATABASE_URL
# Check security group rules
# Verify SSL configuration
```

**High Memory Usage:**
```bash
# Identify memory leak
kubectl top pods

# Increase memory limit
kubectl set resources deployment/{{APP_NAME}} \
  --limits=memory=1Gi \
  --requests=memory=512Mi
```

**Slow Response Times:**
```bash
# Check metrics
- Database query time
- External API calls
- CPU usage

# Solutions:
- Optimize slow queries
- Add caching
- Scale horizontally
```

### Useful Commands

```bash
# Kubernetes
kubectl get pods
kubectl logs -f deployment/{{APP_NAME}}
kubectl describe pod {{POD_NAME}}
kubectl exec -it {{POD_NAME}} -- /bin/sh
kubectl rollout undo deployment/{{APP_NAME}}

# Docker
docker logs -f {{CONTAINER_ID}}
docker exec -it {{CONTAINER_ID}} /bin/sh
docker stats

# Database
psql -h {{HOST}} -U {{USER}} -d {{DATABASE}}
```

## Cost Optimization

### Right-Sizing

```bash
# Analyze resource usage
kubectl top pods
kubectl top nodes

# Downsize over-provisioned resources
# Use spot instances for non-critical workloads
```

### Reserved Instances

- Reserve instances for steady-state workloads
- Use savings plans for predictable usage
- Review and optimize reserved capacity monthly

### Cost Monitoring

```bash
# AWS Cost Explorer
aws ce get-cost-and-usage --time-period ...

# GCP Billing
gcloud beta billing accounts list

# Track costs by service, environment, team
```

## Maintenance

### Scheduled Maintenance

**Database Maintenance:**
- Weekly: Analyze tables, update statistics
- Monthly: Vacuum databases
- Quarterly: Review and optimize indexes

**Security Updates:**
- Weekly: Check for security patches
- Monthly: Update dependencies
- Quarterly: Rotate secrets

**Backup Verification:**
- Monthly: Test restore process
- Quarterly: Restore to staging environment

## Support & Escalation

### On-Call Rotation

**Primary On-Call:**
- Responds to alerts within 15 minutes
- Triage and initial investigation
- Escalate if unable to resolve

**Secondary On-Call:**
- Backup for primary
- Available for escalation
- Senior engineer

### Escalation Path

```
Alert → Primary On-Call → Secondary On-Call → Team Lead → Engineering Manager
   ↓          ↓                  ↓                  ↓              ↓
 15 min    15 min             15 min             15 min          15 min
```

### Incident Response

1. **Acknowledge Alert:** Within 5 minutes
2. **Assess Severity:** P1, P2, P3, P4
3. **Communicate:** Post in incident channel
4. **Investigate:** Review logs, metrics
5. **Mitigate:** Apply fix or rollback
6. **Resolve:** Confirm service restored
7. **Postmortem:** Document within 48 hours

## Related Documentation

- [ARCHITECTURE.md](./ARCHITECTURE.md) - System architecture
- [DEVELOPMENT.md](./DEVELOPMENT.md) - Development setup
- [TESTING.md](./TESTING.md) - Testing strategy
- [SECURITY.md](./SECURITY.md) - Security considerations