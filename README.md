# Payment Gateway with Multi-Method Processing and Hosted Checkout

## Overview

This project is a **Payment Gateway system** inspired by platforms like Razorpay and Stripe.  
It allows merchants to create payment orders using authenticated APIs and enables customers to complete payments through a **hosted checkout page** supporting **UPI and Card payments**.

The project demonstrates:

- Merchant authentication using API key and secret
- Order and payment lifecycle management
- Multi-method payment processing (UPI and Cards)
- Payment validation logic
- Hosted checkout flow
- Fully Dockerized deployment

---

## Technology Stack

- Backend: Node.js / Express (API)
- Database: PostgreSQL
- Frontend (Dashboard): HTML, CSS, JavaScript
- Checkout Page: HTML, CSS, JavaScript
- Containerization: Docker & Docker Compose

---

## System Architecture

The application runs as four Docker services:

| Service            | Port | Description                              |
| ------------------ | ---- | ---------------------------------------- |
| PostgreSQL         | 5432 | Stores merchants, orders, payments       |
| Backend API        | 8000 | Handles authentication, orders, payments |
| Dashboard Frontend | 3000 | Merchant dashboard                       |
| Checkout Frontend  | 3001 | Hosted checkout page                     |

All services start using **one command**.

---

---

## Prerequisites

Ensure the following are installed:

- Docker
- Docker Compose

---

## Running the Application

From the **project root directory**, run:

```bash
docker-compose up -d --build
```

## Service URLs

Backend API: http://localhost:8000  
Dashboard Frontend: http://localhost:3000  
Checkout Page: http://localhost:3001

---

## Health Check

Verify backend and database status:

GET http://localhost:8000/health

Expected response:

```json
{
  "status": "healthy",
  "database": "connected",
  "timestamp": "2024-01-15T10:30:00Z"
}
```

---

## Test Merchant (Auto-Seeded)

A test merchant is automatically created on application startup.

### Test Merchant Credentials

Email: test@example.com  
API Key: key_test_abc123  
API Secret: secret_test_xyz789

Verification endpoint:

GET http://localhost:8000/api/v1/test/merchant

Expected response:

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "email": "test@example.com",
  "api_key": "key_test_abc123",
  "seeded": true
}
```

---

## Dashboard Frontend (Port 3000)

Open in browser:

http://localhost:3000

### Available Pages

Login Page  
Dashboard Home (API credentials & statistics)  
Transactions Page (payment list)

Login uses the test merchant email.  
Password validation is not required for this deliverable.

---

## 🧪 Test Merchant (Auto-Seeded)

A test merchant is automatically created on startup for quick testing.

### 🔑 Dashboard Login

- URL: http://localhost:3000/login.html
- Email: test@example.com
- Password: any value

---

## Hosted Checkout Page (Port 3001)

Checkout URL format:

http://localhost:3001/?order_id=ORDER_ID

### Checkout Flow

Fetch order details using order_id  
Display order summary  
Select payment method (UPI or Card)  
Enter payment details  
Show processing state  
Display success or failure result

---

## API Endpoints

### Health Check

GET /health

### Create Order (Authenticated)

POST /api/v1/orders

Headers:
X-Api-Key  
X-Api-Secret

### Get Order

GET /api/v1/orders/{order_id}

### Create Payment

POST /api/v1/payments

Supports:
UPI payments  
Card payments

### Public Checkout APIs

GET /api/v1/orders/{order_id}/public  
POST /api/v1/payments/public  
GET /api/v1/payments/{payment_id}/public

---

## Payment Validation Logic

### UPI Validation

Validates VPA format using regex  
Rejects invalid VPAs

### Card Validation

Luhn algorithm for card number validation  
Expiry date validation  
Card network detection (Visa, Mastercard, Amex, RuPay)  
Stores only last 4 digits of card number

---

## Payment Status Flow

processing → success  
processing → failed

Payments are created directly in the processing state.

---

## Test Mode (Evaluation Support)

Test mode is controlled via environment variables:

```env
TEST_MODE=true
TEST_PAYMENT_SUCCESS=true
TEST_PROCESSING_DELAY=1000
```

When enabled:

Payment results are deterministic  
Processing delay is fixed  
Suitable for automated evaluation

---

## Database Schema

### Tables

merchants  
orders  
payments

### Relationships

One merchant → many orders  
One order → many payments

Indexes are implemented as required.

---

## Screenshots & Demo

Login page screenshot  
Dashboard home screenshot  
Transactions list screenshot  
Checkout page screenshots (UPI, Card, Processing, Success/Failure)  
Demo video (2–3 minutes) showing complete payment flow

---

## Submission Checklist

Docker Compose starts all services successfully  
Test merchant is auto-seeded  
APIs return correct status codes  
Frontend uses exact data-test-id attributes  
Hosted checkout flow works end-to-end  
README documentation included

---

## Conclusion

This project demonstrates the core functionality of a real-world payment gateway, including secure API authentication, payment validation, multi-method payment processing, and a hosted checkout experience. It showcases backend, frontend, database, and containerization skills relevant to fintech and e-commerce systems.
