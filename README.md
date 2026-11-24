# GraphQL Node.js E-Commerce API

[![Node.js](https://img.shields.io/badge/Node.js-20+-339933?logo=node.js&logoColor=white)](https://nodejs.org/)
[![Apollo Server](https://img.shields.io/badge/Apollo_Server-3-311C87?logo=apollographql&logoColor=white)](https://www.apollographql.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-47A248?logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![GraphQL](https://img.shields.io/badge/GraphQL-E10098?logo=graphql&logoColor=white)](https://graphql.org/)
[![License](https://img.shields.io/badge/license-ISC-blue.svg)](LICENSE)

A GraphQL-based e-commerce backend built with Node.js, Apollo Server, MongoDB, Mongoose, and JWT authentication.

## Features

- User registration and login
- JWT authentication
- Product CRUD operations
- Order creation and management
- Product reviews
- MongoDB integration with Mongoose
- Apollo Studio / GraphQL Playground support
- Jest and Supertest unit/integration tests
- Docker support for MongoDB
- Environment-based configuration

## Tech Stack

| Technology | Purpose |
|---|---|
| Node.js | JavaScript runtime |
| Apollo Server | GraphQL server |
| GraphQL | API query language |
| MongoDB | Primary database |
| Mongoose | MongoDB object modeling |
| JWT | Authentication |
| bcrypt.js | Password hashing |
| Jest | Test runner |
| Supertest | HTTP API testing |
| Docker Compose | Local MongoDB container |

## Project Structure

```text
graphql-nodejs-ecom/
├── src/
│   ├── config/
│   │   └── db.js              # MongoDB connection
│   ├── models/
│   │   ├── Order.js
│   │   ├── Product.js
│   │   ├── Review.js
│   │   └── User.js
│   ├── schema/
│   │   ├── resolvers.js       # GraphQL resolvers
│   │   └── typeDefs.js        # GraphQL schema
│   ├── services/
│   │   ├── authService.js
│   │   ├── orderService.js
│   │   ├── productService.js
│   │   └── reviewService.js
│   ├── app.js
│   ├── server.js
│   └── index.js               # Application entry point
├── tests/
│   ├── auth.test.js
│   ├── orders.test.js
│   ├── products.test.js
│   └── reviews.test.js
├── api-ecom.graphql            # Example GraphQL operations
├── docker-compose.yml
├── Dockerfile
├── jest.config.js
├── package.json
└── package-lock.json
```

## Prerequisites

Before getting started, install:

- Node.js 18 or newer
- npm
- MongoDB, or Docker Desktop

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/JawherKl/graphql-nodejs-ecom.git
cd graphql-nodejs-ecom
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure environment variables

Create a `.env` file in the project root:

```env
MONGO_URI=mongodb://localhost:27017/graphql_ecommerce
JWT_SECRET=replace_this_with_a_secure_secret
```

> Keep `JWT_SECRET` private and use a strong, randomly generated value in production.

### 4. Start MongoDB

Using Docker:

```bash
docker compose up -d mongodb
```

MongoDB will be available at:

```text
mongodb://localhost:27017
```

Alternatively, configure `MONGO_URI` to connect to an existing MongoDB deployment.

## Running the API

Start the server:

```bash
npm start
```

Start in development mode:

```bash
npm run dev
```

The GraphQL API is available at:

```text
http://localhost:4000
```

Apollo Server exposes GraphQL Playground or Apollo Sandbox through the same URL.

## Authentication

Protected operations require a JWT in the `Authorization` header.

```http
Authorization: Bearer YOUR_JWT_TOKEN
```

The server reads the token in `src/server.js` and exposes the authenticated user through the GraphQL context.

## API Operations

### Register

```graphql
mutation Register {
  register(
    name: "John Doe"
    email: "john@example.com"
    password: "secure-password"
    role: "user"
  ) {
    user {
      id
      name
      email
      role
    }
    token
  }
}
```

### Login

```graphql
mutation Login {
  login(email: "john@example.com", password: "secure-password") {
    user {
      id
      name
      email
    }
    token
  }
}
```

### Create a Product

```graphql
mutation CreateProduct {
  createProduct(
    name: "Tablet"
    description: "Lenovo Android tablet"
    price: 219.99
  ) {
    id
    name
    description
    price
  }
}
```

### Get All Products

```graphql
query GetProducts {
  getProducts {
    id
    name
    description
    price
  }
}
```

### Get a Product by ID

```graphql
query GetProduct {
  getProductById(id: "MONGODB_OBJECT_ID") {
    id
    name
    description
    price
  }
}
```

### Update a Product

```graphql
mutation UpdateProduct {
  updateProduct(
    id: "MONGODB_OBJECT_ID"
    name: "Updated Laptop"
    price: 2119.99
  ) {
    id
    name
    price
  }
}
```

### Delete a Product

```graphql
mutation DeleteProduct {
  deleteProduct(id: "MONGODB_OBJECT_ID") {
    id
    name
  }
}
```

### Create an Order

```graphql
mutation CreateOrder {
  createOrder(
    products: [
      { productId: "PRODUCT_ID_1", quantity: 2 }
      { productId: "PRODUCT_ID_2", quantity: 1 }
    ]
    totalAmount: 7339.98
  ) {
    id
    status
    totalAmount
    products {
      quantity
      product {
        id
        name
        price
      }
    }
  }
}
```

### Get an Order

```graphql
query GetOrder {
  getOrderById(id: "ORDER_ID") {
    id
    status
    totalAmount
    products {
      quantity
      product {
        id
        name
        price
      }
    }
  }
}
```

### Update an Order Status

```graphql
mutation UpdateOrderStatus {
  updateOrderStatus(id: "ORDER_ID", status: "Shipped") {
    id
    status
  }
}
```

### Create a Review

```graphql
mutation CreateReview {
  createReview(
    productId: "PRODUCT_ID"
    rating: 5
    comment: "Great product!"
  ) {
    id
    rating
    comment
    createdAt
  }
}
```

### Get Product Reviews

```graphql
query GetReviews {
  getReviewsByProduct(productId: "PRODUCT_ID") {
    id
    rating
    comment
    createdAt
  }
}
```

More ready-to-use operations are available in [`api-ecom.graphql`](api-ecom.graphql).

## Testing

Run the complete Jest test suite:

```bash
npm test
```

Run tests in watch mode:

```bash
npm test -- --watch
```

The test suite includes coverage for authentication, products, orders, and reviews.

## Docker

Start only MongoDB:

```bash
docker compose up -d mongodb
```

Stop the containers:

```bash
docker compose down
```

Remove the MongoDB volume and stored data:

```bash
docker compose down -v
```

## Security Notes

- Never commit `.env` or real JWT secrets.
- Use environment-specific secrets for development and production.
- Restrict JWT lifetime and refresh credentials periodically.
- Add request validation and authorization rules before production use.
- Use HTTPS and a secret manager in production deployments.

## Contributing

Contributions are welcome.

1. Fork the repository.
2. Create a feature branch:

   ```bash
   git checkout -b feature/your-feature
   ```

3. Commit your changes:

   ```bash
   git commit -m "Add your feature"
   ```

4. Push the branch:

   ```bash
   git push origin feature/your-feature
   ```

5. Open a pull request.

## License

ISC

## Star History

<a href="https://www.star-history.com/?repos=jawherkl%2Fgraphql-nodejs-ecom&type=date&legend=top-left">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/chart?repos=jawherkl/graphql-nodejs-ecom&type=date&theme=dark&legend=top-left" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/chart?repos=jawherkl/graphql-nodejs-ecom&type=date&legend=top-left" />
   <img alt="Star History Chart" src="https://api.star-history.com/chart?repos=jawherkl/graphql-nodejs-ecom&type=date&legend=top-left" />
 </picture>
</a>
