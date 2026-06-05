# StockFlow — Containerized Inventory & Order Management System

StockFlow is a production-ready full-stack application for managing products, customers, orders, and inventory movement. It is built for the Ethara AI Software Engineer assessment using React, FastAPI, PostgreSQL, Docker, and Docker Compose.

## Features

### Product Management
- Create, view, update-ready, and delete products
- Unique SKU validation
- Price and stock validation
- Low-stock dashboard tracking

### Customer Management
- Create, view, and delete customers
- Unique email validation
- Backend request validation

### Order Management
- Create orders against customers
- Supports product references and quantity ordered
- Backend automatically calculates total amount
- Prevents orders when inventory is insufficient
- Automatically reduces stock after order placement
- Uses database row locking during order creation to reduce overselling risk

### Dashboard
- Total products
- Total customers
- Total orders
- Low-stock products

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React + Vite |
| Backend | Python + FastAPI |
| Database | PostgreSQL |
| ORM | SQLAlchemy |
| Validation | Pydantic |
| Containerization | Docker |
| Orchestration | Docker Compose |

## Project Structure

```txt
stockflow/
├── backend/
│   ├── app/
│   │   ├── core/
│   │   ├── db/
│   │   ├── models/
│   │   ├── routes/
│   │   ├── schemas/
│   │   └── main.py
│   ├── Dockerfile
│   ├── requirements.txt
│   └── .env.example
├── frontend/
│   ├── src/
│   │   ├── services/
│   │   ├── main.jsx
│   │   └── style.css
│   ├── Dockerfile
│   ├── package.json
│   └── .env.example
├── docker-compose.yml
├── .dockerignore
└── README.md
```

## Run Locally with Docker Compose

```bash
docker compose up --build
```

Frontend:

```txt
http://localhost:5173
```

Backend API:

```txt
http://localhost:8000
```

Swagger Docs:

```txt
http://localhost:8000/docs
```

## API Endpoints

### Products

| Method | Endpoint | Description |
|---|---|---|
| POST | `/products` | Create product |
| GET | `/products` | List products |
| GET | `/products/{id}` | Get product by ID |
| PUT | `/products/{id}` | Update product |
| DELETE | `/products/{id}` | Delete product |

### Customers

| Method | Endpoint | Description |
|---|---|---|
| POST | `/customers` | Create customer |
| GET | `/customers` | List customers |
| GET | `/customers/{id}` | Get customer by ID |
| DELETE | `/customers/{id}` | Delete customer |

### Orders

| Method | Endpoint | Description |
|---|---|---|
| POST | `/orders` | Create order |
| GET | `/orders` | List orders |
| GET | `/orders/{id}` | Get order by ID |
| DELETE | `/orders/{id}` | Delete order |

### Dashboard

| Method | Endpoint | Description |
|---|---|---|
| GET | `/dashboard/summary` | Dashboard metrics |

## Sample Order Payload

```json
{
  "customer_id": 1,
  "items": [
    {
      "product_id": 1,
      "quantity": 2
    }
  ]
}
```

## Deployment Plan

### Backend
Recommended: Render or Railway

Set environment variable:

```txt
DATABASE_URL=<your-postgres-url>
CORS_ORIGINS=<your-frontend-url>
```

### Frontend
Recommended: Vercel or Netlify

Set environment variable:

```txt
VITE_API_URL=<your-backend-api-url>
```

### Docker Hub

```bash
docker build -t your-dockerhub-username/stockflow-backend:latest ./backend
docker push your-dockerhub-username/stockflow-backend:latest
```

## Why This Submission Stands Out

Most assessment submissions are simple CRUD apps. StockFlow adds real engineering practices:

- Clean backend architecture
- Strong validation and error handling
- Unique SKU and email constraints
- Transaction-safe inventory reduction
- Multi-table order design using order items
- Fully containerized architecture
- Professional README and deployment-ready environment configuration

## Future Improvements

- JWT authentication
- Product image upload
- Inventory transaction history
- Search and filters
- Order cancellation with stock restoration
- Alembic migration setup
- Unit and integration tests
