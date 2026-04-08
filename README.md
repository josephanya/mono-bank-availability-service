# mono-bank-availability-service

A microservice that monitors bank operational availability by analyzing transaction status codes across different time windows.

## 🛠 Tech Stack

- **Runtime**: Node.js
- **Language**: TypeScript
- **Framework**: Express.js
- **Database**: MongoDB with Mongoose, Redis
- **Scheduling**: node-cron
- **Testing**: Jest with Supertest
- **Logging**: Pino

## 📋 Prerequisites

- Node.js 18+
- MongoDB 5+
- npm or yarn

## 🔧 Installation & Setup

1. **Clone and install dependencies**:
```bash
git clone https://github.com/josephanya/mono-bank-availability-api.git
cd bank-availability-api
npm install
```

2. **Environment Configuration**:
```bash
cp .env.example .env
# Edit .env with your configuration
```

3. **Database Setup**:
Make sure MongoDB is running locally or update `MONGODB_URI` in `.env`. Make sure Redis is running locally or update `REDIS_URL` in `.env`

4. **Build the project**:
```bash
npm run build
```

## 🚀 Running the Service

**Development mode**:
```bash
npm run local
```

**Production mode**:
```bash
npm start
```

**Running with Docker**:
```bash
docker-compose up
```

The service will be available at `http://localhost:3000`

## 📊 API Documentation

### Authentication
All endpoints require an API key in the header:
```bash
x-api-key: your-secure-api-key-here
```

### Endpoints

#### Get All Banks Availability
```bash
GET /banks/availability?window=1h
```

**Parameters**:
- `window` (optional): Time window (`1h`, `6h`, `24h`). Default: `1h`

**Response**:
```json
{
  "data": [
    {
      "bank_nip_code": "NIP001",
      "time_window": "1h",
      "availability_percentage": 95.24,
      "confidence_level": "High",
      "last_calculated_at": "2025-05-25T10:30:00Z"
    }
  ],
  "requested_window": "1h"
}
```

#### Get Specific Bank Availability
```bash
GET /banks/{bank_nip_code}/availability?window=1h
```

**Response**:
```json
{
  "data": {
    "bank_nip_code": "NIP001",
    "time_window": "1h",
    "availability_percentage": 95.24,
    "confidence_level": "High",
    "last_calculated_at": "2025-05-25T10:30:00Z"
  },
  "requested_window": "1h"
}
```

#### Health Check
```bash
GET /health
```

## 🧪 Testing

**Run all tests**:
```bash
npm test
```

## 📊 Example Usage

**Get all banks availability**:
```bash
curl -H "x-api-key: your-api-key" \
  "http://localhost:3000/banks/availability?window=6h"
```

**Get specific bank availability**:
```bash
curl -H "x-api-key: your-api-key" \
  "http://localhost:3000/banks/NIP001/availability"
```

## 🏗 Architecture & Design Decisions

### 📈 Scalability Considerations
- **Database Optimization**: indexing for fast queries
- **Caching Ready**: Redis integration 
- **Response Caching**: API responses are cached with short TTL

### Message Queue Integration
- The service could subscribe to a messaging queue and process events (new transactions) as they come in.

---

**Built with ❤️**
