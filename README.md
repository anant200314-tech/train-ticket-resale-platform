# 🚆 Train Ticket Resale Platform

> A complete C2C (Customer-to-Customer) train ticket resale marketplace platform built with modern web technologies.

## 📋 Project Overview

Train Ticket Resale Platform is a full-stack web application that allows users to:
- **List** train tickets they cannot use for resale
- **Search** and **purchase** tickets from other users
- **Manage** their listings and orders
- **Make secure payments** via Razorpay
- **Chat** with buyers/sellers in-app
- **Review and rate** transactions

## 🏗️ Tech Stack

### Backend
- **Node.js + Express.js** - RESTful API server
- **PostgreSQL** - Relational database
- **Redis** - Caching & session management (optional)
- **JWT** - Authentication & security
- **Multer** - File uploads
- **Nodemailer** - Email notifications

### Frontend
- **React.js** - UI library
- **Next.js** - Framework for optimization
- **Redux/Zustand** - State management
- **Tailwind CSS** - Styling
- **Axios** - HTTP client

### Infrastructure
- **Railway** - Backend hosting (recommended for India)
- **Vercel** - Frontend hosting
- **AWS S3** - File storage
- **Razorpay** - Payment gateway
- **Gmail SMTP** - Email service

## 🚀 Quick Start

### Prerequisites
- Node.js 16+
- PostgreSQL 12+
- Git
- npm/yarn

### Backend Setup

```bash
# Clone repository
git clone <repository-url>
cd train-ticket-resale-platform/backend

# Install dependencies
npm install

# Create environment file
cp .env.example .env

# Update .env with your credentials
# See .env.example for all required variables

# Run database migrations
npm run migrate

# Start development server
npm run dev
```

### Frontend Setup

```bash
# Navigate to frontend
cd ../frontend

# Install dependencies
npm install

# Create environment file
echo 'REACT_APP_API_URL=http://localhost:5000/api' > .env.local

# Start development server
npm start
```

The application will open at `http://localhost:3000`

## 📊 Project Structure

```
train-ticket-resale-platform/
├── backend/                 # Node.js + Express backend
│   ├── models/             # Database models
│   ├── routes/             # API routes
│   ├── controllers/        # Business logic
│   ├── middleware/         # Custom middleware
│   └── server.js           # Entry point
├── frontend/               # React frontend
│   ├── src/
│   │   ├── components/     # React components
│   │   ├── pages/          # Page components
│   │   ├── context/        # React context
│   │   └── styles/         # CSS files
│   └── public/             # Static assets
├── DEPLOYMENT_GUIDE.md     # Production deployment guide
├── railway.json            # Railway configuration
├── .env.example            # Environment variables template
└── README.md               # This file
```

## 🔑 Key Features

### User Authentication
- Email & password signup/login
- OTP verification
- Password reset functionality
- JWT-based session management

### Ticket Management
- List tickets with details (date, train, class, price)
- Upload ticket image proof
- Real-time search & filtering
- Admin approval workflow
- Automatic listing expiry

### Payment System
- Secure Razorpay integration
- Escrow payment handling
- Transaction history
- Automatic refunds

### Messaging
- In-app chat between buyers/sellers
- Masked contact details
- Message history
- Real-time notifications

### Admin Dashboard
- User verification
- Ticket approval/rejection
- Dispute resolution
- Analytics & reports
- Revenue tracking

## 🚢 Deployment

### Quick Deployment Steps

1. **Frontend Deployment (Vercel)**
   ```bash
   # Push code to GitHub
   git push origin main
   
   # Connect Vercel to GitHub repository
   # Select frontend directory
   # Auto-deploys on every push
   ```

2. **Backend Deployment (Railway)**
   ```bash
   # Connect Railway to GitHub
   # Auto-deploys backend with PostgreSQL
   # Set environment variables in Railway dashboard
   ```

For detailed deployment instructions, see [DEPLOYMENT_GUIDE.md](./DEPLOYMENT_GUIDE.md)

## 🔒 Security Features

- ✅ HTTPS/SSL encryption
- ✅ Password hashing with bcrypt
- ✅ JWT token validation
- ✅ SQL injection prevention
- ✅ XSS protection
- ✅ CORS configuration
- ✅ Rate limiting
- ✅ Input validation
- ✅ Environment variable protection

## 💰 Monetization Model

- **Listing Fee**: ₹50 per ticket listing (or 5% commission)
- **Featured Listings**: ₹200 for 7 days visibility
- **Subscription**: Premium seller plans
- **Ads**: Optional sponsored listings

## 📈 Scalability

### Phase 1 (MVP - 0-1000 users)
- Single Railway instance
- PostgreSQL on Railway
- S3 for file storage
- Estimated cost: $10-20/month

### Phase 2 (Growth - 1000-10k users)
- Multiple Railway instances
- Redis caching
- CDN for static assets
- Estimated cost: $50-100/month

### Phase 3 (Scale - 10k+ users)
- Load balancer
- Database replication
- Microservices architecture
- Message queues
- Estimated cost: $500-1000/month

## ⚠️ Important Notes

### Legal Compliance
- Check local railway authority rules on ticket resale
- Add clear Terms & Conditions
- Include necessary disclaimers
- Implement KYC verification
- Follow GDPR for data protection

### Testing
- Test all payment flows with Razorpay test keys first
- Test email notifications
- Load test before production
- Test file upload functionality

## 🤝 Contributing

Contributions welcome! Please follow the GitHub workflow:
1. Fork repository
2. Create feature branch
3. Make changes
4. Submit pull request

## 📞 Support & Contact

For issues and questions:
- Open GitHub issue
- Email: support@example.com
- Visit: https://example.com/support

## 📄 License

MIT License - feel free to use for personal/commercial projects

## 🙏 Acknowledgments

- Railway for excellent hosting platform
- Razorpay for payment infrastructure
- React community for amazing tools

---

**Built with ❤️ for the Indian startup ecosystem**

**Last Updated:** December 2025
