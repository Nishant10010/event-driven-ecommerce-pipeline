# Event-Driven E-Commerce Order Pipeline

> A production-ready, event-driven microservices architecture for e-commerce order processing using Node.js, Kafka, MongoDB, and React.

## 🎯 Overview

This system implements a robust, fault-tolerant order processing pipeline with:
- **Event-driven architecture** using Apache Kafka for asynchronous communication
- **Microservices** for Order, Inventory, Payment, and Shipping domains
- **Saga pattern** for distributed transactions and compensation
- **Idempotency** and exactly-once semantics (practical)
- **Real-time dashboard** with live order status tracking
- **Observability** with distributed tracing, structured logging, and metrics

## 🏗️ Architecture

### System Flow
```
Order Created → Inventory Reserved → Payment Authorized → Order Shipped
     ↓               ↓                    ↓                   ↓
   Event          Event                Event              Event
     ↓               ↓                    ↓                   ↓
  Kafka           Kafka                Kafka              Kafka
     ↓               ↓                    ↓                   ↓
 MongoDB         MongoDB              MongoDB            MongoDB
```

### Services

1. **Order Service** - Creates and manages orders
2. **Inventory Service** - Reserves/releases inventory
3. **Payment Service** - Processes payments and authorizations
4. **Shipping Service** - Handles order fulfillment
5. **React Dashboard** - Real-time order status visualization

## 🚀 Key Features

### Event Model & Topics
- ✅ Canonical event schemas (OrderCreated, InventoryReserved, PaymentAuthorized, etc.)
- ✅ Versioned schemas with correlation and causation IDs
- ✅ Event sourcing with append-only log

### Kafka Integration
- ✅ Partitioned by orderId for ordering guarantees
- ✅ Producer delivery guarantees (acks=all)
- ✅ Consumer groups for parallel processing
- ✅ Dead Letter Queue (DLQ) for failed messages

### Reliability & Fault Tolerance
- ✅ Idempotency keys and deduplication store
- ✅ Retry logic with exponential backoff
- ✅ Circuit breakers for downstream services
- ✅ Saga/compensation on failures

### Observability
- ✅ Structured JSON logging
- ✅ Distributed tracing with correlation IDs
- ✅ Metrics (lag, latency, throughput)
- ✅ Health checks for all services

### Security
- ✅ JWT-protected endpoints
- ✅ Payload validation (Zod/Joi)
- ✅ PII minimization in events

## 📁 Project Structure

```
event-driven-ecommerce-pipeline/
├── services/
│   ├── order-service/
│   ├── inventory-service/
│   ├── payment-service/
│   └── shipping-service/
├── dashboard/
│   └── react-dashboard/
├── infrastructure/
│   ├── docker-compose.yml
│   ├── kafka/
│   └── mongodb/
├── shared/
│   ├── events/
│   ├── schemas/
│   └── utils/
├── tests/
│   ├── contract/
│   ├── integration/
│   └── chaos/
└── docs/
    ├── architecture.md
    ├── api.md
    └── runbook.md
```

## 🛠️ Tech Stack

- **Backend**: Node.js + Express
- **Message Broker**: Apache Kafka
- **Database**: MongoDB
- **Frontend**: React + TypeScript
- **Real-time**: WebSocket/SSE
- **Validation**: Zod
- **Testing**: Jest, Supertest
- **Containerization**: Docker + Docker Compose

## 🚦 Getting Started

### Prerequisites
- Docker & Docker Compose
- Node.js 18+
- npm or yarn

### Quick Start

```bash
# Clone the repository
git clone https://github.com/Nishant10010/event-driven-ecommerce-pipeline.git
cd event-driven-ecommerce-pipeline

# Start all services with Docker Compose
make up
# OR
docker-compose up -d

# Install dependencies for all services
make install

# Run database migrations and seed data
make seed

# Start development
make dev
```

### Access Points
- **React Dashboard**: http://localhost:3000
- **Order Service API**: http://localhost:4001
- **Inventory Service API**: http://localhost:4002
- **Payment Service API**: http://localhost:4003
- **Shipping Service API**: http://localhost:4004
- **Kafka UI**: http://localhost:8080
- **MongoDB**: localhost:27017

## 📊 Event Schemas

### OrderCreated
```json
{
  "eventId": "uuid",
  "eventType": "OrderCreated",
  "version": "1.0",
  "timestamp": "2025-11-11T14:00:00Z",
  "correlationId": "uuid",
  "causationId": "uuid",
  "payload": {
    "orderId": "string",
    "customerId": "string",
    "items": [],
    "totalAmount": "number"
  }
}
```

## 🔄 Saga Flow

### Happy Path
1. Order Service publishes `OrderCreated`
2. Inventory Service reserves stock → `InventoryReserved`
3. Payment Service authorizes payment → `PaymentAuthorized`
4. Shipping Service ships order → `OrderShipped`

### Compensation Flow
If payment fails:
1. Payment Service publishes `PaymentFailed`
2. Inventory Service releases stock → `InventoryReleased`
3. Order Service updates status → `OrderCancelled`

## 🧪 Testing

```bash
# Run unit tests
make test

# Run integration tests
make test:integration

# Run contract tests
make test:contract

# Run chaos tests
make test:chaos

# Generate load
make load-test
```

## 📈 Monitoring

- **Logs**: Structured JSON logs in `./logs/`
- **Traces**: Correlation IDs in headers
- **Metrics**: Prometheus-compatible endpoints at `/metrics`

## 🔧 Configuration

Environment variables are configured in `.env` files:

```env
# Kafka
KAFKA_BROKERS=localhost:9092
KAFKA_CLIENT_ID=order-service

# MongoDB
MONGO_URI=mongodb://localhost:27017/orders

# Service
PORT=4001
NODE_ENV=development

# Security
JWT_SECRET=your-secret-key
```

## 🤝 Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## 📝 License

MIT License - see [LICENSE](LICENSE) file for details.

## 📚 Documentation

- [Architecture Guide](docs/architecture.md)
- [API Documentation](docs/api.md)
- [Runbook](docs/runbook.md)
- [Development Guide](docs/development.md)

## 🐛 Troubleshooting

See [TROUBLESHOOTING.md](TROUBLESHOOTING.md) for common issues and solutions.

---

**Built with ❤️ for learning event-driven architectures**
