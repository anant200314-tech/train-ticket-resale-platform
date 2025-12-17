# 🚆 Train Ticket Resale Platform - Complete Deployment Guide

## Phase 1: Local Development Setup

### Prerequisites
- Node.js 16+ and npm
- PostgreSQL 12+
- Git
- AWS Account (for S3)
- Razorpay Account (for payments)

### Step 1: Clone and Setup Backend

```bash
# Clone repository
git clone https://github.com/anant200314-tech/train-ticket-resale-platform.git
cd train-ticket-resale-platform/backend

# Install dependencies
npm install

# Create .env file
cp .env.example .env

# Update .env with your credentials
DB_USER=postgres
DB_PASSWORD=your_password
DB_HOST=localhost
DB_PORT=5432
DB_NAME=train_resale
JWT_SECRET=your_super_secret_key
RAZORPAY_KEY_ID=your_key
RAZORPAY_KEY_SECRET=your_secret

# Run database migrations
npm run migrate

# Start backend server
npm run dev
```

### Step 2: Setup Frontend

```bash
# Navigate to frontend
cd ../frontend

# Install dependencies
npm install

# Create .env.local
REACT_APP_API_URL=http://localhost:5000/api

# Start frontend
npm start
```

## Phase 2: Deployment to Vercel (Frontend)

### Step 1: Connect GitHub to Vercel
1. Go to vercel.com and sign up
2. Click "New Project"
3. Import the GitHub repository
4. Select the `frontend` directory as the root

### Step 2: Add Environment Variables
In Vercel Dashboard:
- Go to Settings > Environment Variables
- Add: `REACT_APP_API_URL` = `https://your-backend-domain.com/api`

### Step 3: Deploy
- Vercel auto-deploys on every push to main
- Frontend URL: https://your-app-name.vercel.app

## Phase 3: Deployment to Railway/Render (Backend)

### Using Railway (Recommended for Indians)

#### Step 1: Connect Repository
1. Go to railway.app
2. Click "New Project"
3. Select "Deploy from GitHub"
4. Connect your GitHub account
5. Select the repository

#### Step 2: Configure Backend
```bash
# In Railway dashboard:
1. Add service "Node.js"
2. Connect to your GitHub repo
3. Set root directory: ./backend
```

#### Step 3: Add PostgreSQL Database
1. Click "+New" > Database > PostgreSQL
2. Railway auto-creates database
3. Copy DATABASE_URL from PostgreSQL service

#### Step 4: Set Environment Variables
In Railway Dashboard > Variables:
```
NODE_ENV=production
PORT=8000
DB_URL=postgres://...(from Railway)
JWT_SECRET=your_strong_secret
RAZORPAY_KEY_ID=your_key
RAZORPAY_KEY_SECRET=your_secret
AWS_ACCESS_KEY=your_aws_key
AWS_SECRET_KEY=your_secret
AWS_S3_BUCKET=train-resale-tickets
AWS_REGION=ap-south-1
```

#### Step 5: Deploy
- Push to GitHub → Auto-deploys
- Backend URL: https://your-backend-url.up.railway.app

### Alternative: Using Render

1. Go to render.com
2. Create New > Web Service
3. Connect GitHub repository
4. Settings:
   - Runtime: Node
   - Build Command: `cd backend && npm install`
   - Start Command: `cd backend && npm start`
5. Add PostgreSQL instance
6. Set environment variables
7. Deploy

## Phase 4: AWS S3 Setup (File Uploads)

### Step 1: Create S3 Bucket
```bash
# AWS Console > S3
1. Create bucket: train-resale-tickets
2. Region: ap-south-1
3. Block Public Access: OFF
4. Enable versioning
```

### Step 2: Create IAM User
```bash
# AWS Console > IAM
1. Create user: train-resale-app
2. Attach policy: AmazonS3FullAccess
3. Create access key
4. Copy Access Key ID and Secret
```

### Step 3: Configure CORS
```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "PUT", "POST"],
    "AllowedOrigins": ["https://yourdomain.com"],
    "MaxAgeSeconds": 3000
  }
]
```

## Phase 5: Database Backups

### Auto-backup with Railway
```bash
# Railway handles automatic daily backups
# Access in Railway Dashboard > PostgreSQL > Backups
```

### Manual Backup
```bash
# Connect to Railway PostgreSQL
psql $DATABASE_URL

# Backup command
pg_dump $DATABASE_URL > backup.sql

# Restore
psql $DATABASE_URL < backup.sql
```

## Phase 6: Domain & SSL Setup

### Vercel (Frontend)
1. Vercel Dashboard > Settings > Domains
2. Add your domain
3. Update DNS records
4. SSL auto-issued

### Railway (Backend)
1. Railway Dashboard > Settings > Custom Domains
2. Add domain
3. Update DNS records
4. SSL auto-issued

## Phase 7: Monitoring & Logging

### Railway Logs
```bash
# Real-time logs
railway logs

# Deployment logs
railway status
```

### Vercel Analytics
- Dashboard shows build times, deployments, errors
- Real-time monitoring available

## Phase 8: Payment Gateway (Razorpay) Live Setup

### Step 1: Get Live Credentials
1. Login to Razorpay dashboard
2. Settings > API Keys
3. Copy Live Key ID and Secret

### Step 2: Update Environment
```bash
# In production .env
RAZORPAY_KEY_ID=rzp_live_xxxxx
RAZORPAY_KEY_SECRET=xxxx
```

### Step 3: Setup Webhooks
1. Razorpay Dashboard > Settings > Webhooks
2. Add endpoint: `https://backend.yourdomain.com/api/payments/webhook`
3. Events: payment.authorized, payment.failed

## Phase 9: Email Configuration (Gmail SMTP)

### Step 1: Generate App Password
1. Google Account > Security
2. 2FA enabled
3. App passwords > Mail > Windows Device
4. Copy 16-char password

### Step 2: Update .env
```
EMAIL_USER=your_email@gmail.com
EMAIL_PASSWORD=xxxx xxxx xxxx xxxx
```

## Phase 10: CI/CD Pipeline (GitHub Actions)

### Create `.github/workflows/deploy.yml`
```yaml
name: Deploy
on:
  push:
    branches: [main]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: cd backend && npm install && npm run test
      - run: cd frontend && npm install && npm run build
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying to Railway..."
```

## Phase 11: Security Checklist

- [x] HTTPS enabled on all domains
- [x] Environment variables never committed
- [x] SQL injection prevention (parameterized queries)
- [x] XSS protection (React escaping)
- [x] CORS configured properly
- [x] Rate limiting enabled
- [x] Input validation on all endpoints
- [x] Password hashing with bcrypt
- [x] JWT token validation
- [x] Sensitive data not logged

## Phase 12: Production Checklist

### Pre-Launch
- [x] Database backups configured
- [x] Error monitoring setup (Sentry recommended)
- [x] Email service tested
- [x] Payment gateway tested
- [x] File uploads tested
- [x] Load testing completed
- [x] Security audit done
- [x] Performance optimization done

### Post-Launch
- Monitor server health
- Track user signups and tickets
- Monitor payment success rates
- Check error logs daily
- Respond to user support requests

## Troubleshooting

### Backend won't start
```bash
# Check if PORT is in use
lsof -i :8000

# Check database connection
psql $DATABASE_URL

# View logs
railway logs
```

### Frontend not connecting to API
```bash
# Check CORS settings
# Verify API_URL in .env
# Check browser console for errors
```

### Payments failing
```bash
# Verify Razorpay credentials
# Check webhook URL is accessible
# Check payment logs in Razorpay dashboard
```

## Cost Estimation

| Service | Price | Notes |
|---------|-------|-------|
| Railway Backend | $5-20/month | Generous free tier |
| Vercel Frontend | Free | Very generous free tier |
| PostgreSQL (Railway) | Included | 1GB free |
| AWS S3 | $0.023/GB stored | First 1GB free tier |
| Domain | $10-15/year | .com, .in |
| Email (Gmail) | Free | Limited to 500/day |
| **Total** | **~$30-50/month** | Scalable |

## Scaling Strategy

### Phase 1 (0-1000 users)
- Single Railway instance
- PostgreSQL on Railway
- S3 for files

### Phase 2 (1000-10,000 users)
- Multiple Railway instances
- Redis caching
- CDN for assets

### Phase 3 (10,000+ users)
- Load balancer
- Database replication
- Message queues (RabbitMQ)
- Microservices architecture

## Next Steps

1. Update credentials in .env files
2. Push code to GitHub
3. Deploy frontend to Vercel
4. Deploy backend to Railway
5. Test all endpoints
6. Monitor logs for errors
7. Launch!

Good luck with your launch! 🚀
