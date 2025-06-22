# Express.js Product API

A RESTful API for managing products built with Express.js, featuring CRUD operations, authentication, validation, filtering, pagination, and search functionality.

## Installation

1. Install dependencies:
   ```bash
   npm install express body-parser uuid
   ```

2. Start the server:
   ```bash
   node server.js
   ```

The server will start on `http://localhost:3000`

## Authentication

All API endpoints require an API key to be included in the request headers:

```
x-api-key: Zhen2025
```

## API Endpoints

### Root Endpoint
- **GET** `/` - Welcome message
- **Response:** `"Welcome to the Product API! Go to /api/products to see all products."`

### Products CRUD Operations

#### Get All Products
- **GET** `/api/products`
- Returns array of all products

#### Get Product by ID
- **GET** `/api/products/:id`
- Returns specific product or 404 if not found

#### Create New Product
- **POST** `/api/products`
- **Headers:** `Content-Type: application/json`
- **Body:**
  ```json
  {
    "name": "Product Name",
    "description": "Product description",
    "price": 99.99,
    "category": "electronics",
    "inStock": true
  }
  ```

#### Update Product
- **PUT** `/api/products/:id`
- **Headers:** `Content-Type: application/json`
- **Body:** Same as create (all fields required)

#### Delete Product
- **DELETE** `/api/products/:id`
- Returns deleted product data

### Advanced Features

#### Filter Products by Category (with Pagination)
- **GET** `/api/products/filter`
- **Query Parameters:**
  - `category` (optional): Filter by product category
  - `page` (optional, default: 1): Page number
  - `limit` (optional, default: 10): Items per page

**Example:** `/api/products/filter?category=electronics&page=1&limit=5`

#### Search Products by Name
- **GET** `/api/products/search`
- **Query Parameters:**
  - `name` (required): Search term (case-insensitive partial match)

**Example:** `/api/products/search?name=laptop`

#### Get Product Statistics
- **GET** `/api/products/stats`
- Returns count of products by category

## Sample Data

The API comes with 3 pre-loaded products:

1. **Laptop** (ID: 1) - Electronics, $1200, In Stock
2. **Smartphone** (ID: 2) - Electronics, $800, In Stock  
3. **Coffee Maker** (ID: 3) - Kitchen, $50, Out of Stock

## Request/Response Examples

### Create Product Request
```bash
curl -X POST http://localhost:3000/api/products \
  -H "Content-Type: application/json" \
  -H "x-api-key: Zhen2025" \
  -d '{
    "name": "Wireless Headphones",
    "description": "Noise-cancelling wireless headphones",
    "price": 199.99,
    "category": "electronics",
    "inStock": true
  }'
```

### Response
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Wireless Headphones",
  "description": "Noise-cancelling wireless headphones",
  "price": 199.99,
  "category": "electronics",
  "inStock": true
}
```

### Filter Products Response
```json
{
  "page": 1,
  "total": 2,
  "products": [
    {
      "id": "1",
      "name": "Laptop",
      "description": "High-performance laptop with 16GB RAM",
      "price": 1200,
      "category": "electronics",
      "inStock": true
    }
  ]
}
```

### Statistics Response
```json
{
  "electronics": 2,
  "kitchen": 1
}
```

## Error Handling

The API returns appropriate HTTP status codes and error messages:

- **400** - Bad Request (validation errors)
- **403** - Forbidden (invalid API key)
- **404** - Not Found (product not found)
- **500** - Internal Server Error

### Error Response Format
```json
{
  "error": "Error message description"
}
```

## Middleware Features

- **Request Logger**: Logs all incoming requests with timestamp, method, and URL
- **API Key Authentication**: Validates `x-api-key` header on all requests
- **JSON Body Parser**: Parses JSON request bodies
- **Product Validation**: Validates required fields for create/update operations
- **Global Error Handler**: Catches and formats all errors consistently

## Environment Variables

- `PORT`: Server port (default: 3000)
- `API_KEY`: API key for authentication (default: "Zhen2025")