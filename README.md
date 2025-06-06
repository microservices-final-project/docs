# **Solution Proposal**

---

## ⚙️ **Deployment Strategy**

### 🔹 Personal Development Environment

* Active branch: `develop`
* Used for local development and preliminary testing of features.

### 🔹 Cloud Environments

* **Stage:** Deployment environment for full system testing (E2E, performance).
    * Active branch: `stage`
* **Production:** Final deployment environment serving real users.
    * Active branch: `main`

---

## 🔁 **Microservices Development Workflow**

### 🔸 Branching Strategy

* Feature and bugfix branches:

  * `feature/<feature-name>`
  * `fix/<issue-description>`

---

### 🔸 Pull Request to `develop`

Upon opening a PR:

* ✅ Static code analysis via **SonarQube**
* ✅ Dependency vulnerability scan with **Trivy**
* ✅ Execution of:

  * Unit tests
  * Integration tests
* ✅ Manual approval required

📦 **Result**:
If all checks pass and manual approval is granted → **Merge to `develop`**

---

### 🔸 Pull Request to `stage`

* 🚀 Temporary image deployed to **Stage Cloud Environment**
* ✅ Execution of:

  * End-to-End (E2E) tests
  * Performance/Load tests

📦 **Result**:

* ✅ **Success** → Merge to `stage` and create a **consolidated image**
* ❌ **Failure** → Cancel PR and **revert deployed image**

---

### 🔸 Pull Request to `main`

* ✅ Requires **manual approval**
* 🚀 Final image is deployed to the **Production Cloud Environment**

📦 **Result**:
Successful merge → **Production release**

---

## 📈 **Monitoring & Observability Stack**

| Environment | Prometheus | Grafana | ELK Stack (Elasticsearch, Logstash, Kibana) | Zipkin |
| ----------- | ---------- | ------- | ------------------------------------------- | ------ |
| `develop`   | ❌          | ❌       | ❌                                           | ✅      |
| `stage`     | ✅          | ✅       | ✅                                           | ✅      |
| `prod`      | ✅          | ✅       | ✅                                           | ✅      |

---

### 🔍 Monitoring Strategy by Technology

* **Prometheus**: Metric scraping for system health (CPU, memory, requests).
* **Grafana**: Dashboards for visualizing service metrics and KPIs.
* **ELK Stack**: Centralized logging and log analysis.
* **Zipkin**: Distributed tracing for identifying performance bottlenecks.

### 📊 Custom Dashboards

* Technical metrics: request rate, latency, errors, CPU, memory.
* Business metrics: sign-ups, matches played, transactions, etc.

### 🚨 Alerts & Probes

* **Alerting system**: Set up with Prometheus AlertManager and/or Grafana.
* **Health checks**: Liveness and readiness probes configured per microservice.
* **Distributed tracing**: Zipkin integrated via HTTP headers for service tracing.
* **Notification channels** CI/CD pipelines send automated email notifications.

# Microservices Description

## Endpoints

### **user-service/**

| Method | Endpoint                               | Description                     | Request Body           | Response   Type                |
| ------ | -------------------------------------- | ------------------------------- | ---------------------- | ------------------------------ |
| GET    | `/api/address`                         | Get all addresses               | –                      | List of `AddressDto`           |
| GET    | `/api/address/{addressId}`             | Get address by ID               | –                      | `AddressDto`                   |
| POST   | `/api/address`                         | Create a new address            | `AddressDto`           | `AddressDto`                   |
| PUT    | `/api/address`                         | Update an address               | `AddressDto`           | `AddressDto`                   |
| PUT    | `/api/address/{addressId}`             | Update address by ID            | `AddressDto`           | `AddressDto`                   |
| DELETE | `/api/address/{addressId}`             | Delete address by ID            | –                      | `true` if successful           |
| GET    | `/api/credentials`                     | Get all credentials             | –                      | List of `CredentialDto`        |
| GET    | `/api/credentials/{credentialId}`      | Get credential by ID            | –                      | `CredentialDto`                |
| GET    | `/api/credentials/username/{username}` | Get credential by username      | –                      | `CredentialDto`                |
| POST   | `/api/credentials`                     | Create a new credential         | `CredentialDto`        | `CredentialDto`                |
| PUT    | `/api/credentials`                     | Update a credential             | `CredentialDto`        | `CredentialDto`                |
| DELETE | `/api/credentials/{credentialId}`      | Delete credential by ID         | –                      | `true` if successful           |
| GET    | `/api/user`                            | Get all users                   | –                      | List of `UserDto`              |
| GET    | `/api/user/{userId}`                   | Get user by ID                  | –                      | `UserDto`                      |
| POST   | `/api/user`                            | Create a new user               | `UserDto`              | `UserDto`                      |
| PUT    | `/api/user`                            | Update a user                   | `UserDto`              | `UserDto`                      |
| PUT    | `/api/user/{userId}`                   | Update user by ID               | `UserDto`              | `UserDto`                      |
| DELETE | `/api/user/{userId}`                   | Delete user by ID               | –                      | `true` if successful           |
| GET    | `/api/verification-token`              | Get all verification tokens     | –                      | List of `VerificationTokenDto` |
| GET    | `/api/verification-token/{tokenId}`    | Get verification token by ID    | –                      | `VerificationTokenDto`         |
| POST   | `/api/verification-token`              | Create a new verification token | `VerificationTokenDto` | `VerificationTokenDto`         |
| PUT    | `/api/verification-token`              | Update a verification token     | `VerificationTokenDto` | `VerificationTokenDto`         |
| PUT    | `/api/verification-token/{tokenId}`    | Update token by ID              | `VerificationTokenDto` | `VerificationTokenDto`         |
| DELETE | `/api/verification-token/{tokenId}`    | Delete token by ID              | –                      | `true` if successful           |

### **product-service/**

| Method | Endpoint                       | Description                 | Request Body  | Response Type        |
| ------ | ------------------------------ | --------------------------- | ------------- | -------------------- |
| GET    | `/api/products`                | Get all products            | –             | `List<ProductDto>`   |
| GET    | `/api/products/{productId}`    | Get product by ID           | –             | `ProductDto`         |
| POST   | `/api/products`                | Create a new product        | `ProductDto`  | `ProductDto`         |
| PUT    | `/api/products`                | Update an existing product  | `ProductDto`  | `ProductDto`         |
| PUT    | `/api/products/{productId}`    | Update product by ID        | `ProductDto`  | `ProductDto`         |
| DELETE | `/api/products/{productId}`    | Delete product by ID        | –             | `true` if successful |
| GET    | `/api/categories`              | Get all categories          | –             | `List<CategoryDto>`  |
| GET    | `/api/categories/{categoryId}` | Get category by ID          | –             | `CategoryDto`        |
| POST   | `/api/categories`              | Create a new category       | `CategoryDto` | `CategoryDto`        |
| PUT    | `/api/categories`              | Update an existing category | `CategoryDto` | `CategoryDto`        |
| PUT    | `/api/categories/{categoryId}` | Update category by ID       | `CategoryDto` | `CategoryDto`        |
| DELETE | `/api/categories/{categoryId}` | Delete category by ID       | –             | `true` if successful |

### **order-service/**

| Method | Endpoint                | Description        | Request Body | Response Type    |
| ------ | ----------------------- | ------------------ | ------------ | ---------------- |
| GET    | `/api/carts`            | Get all carts      | –            | `List<CartDto>`  |
| GET    | `/api/carts/{cartId}`   | Get cart by ID     | –            | `CartDto`        |
| POST   | `/api/carts`            | Create a new cart  | `CartDto`    | `CartDto`        |
| PUT    | `/api/carts`            | Update a cart      | `CartDto`    | `CartDto`        |
| PUT    | `/api/carts/{cartId}`   | Update cart by ID  | `CartDto`    | `CartDto`        |
| DELETE | `/api/carts/{cartId}`   | Delete cart by ID  | –            | `boolean`        |
| GET    | `/api/orders`           | Get all orders     | –            | `List<OrderDto>` |
| GET    | `/api/orders/{orderId}` | Get order by ID    | –            | `OrderDto`       |
| POST   | `/api/orders`           | Create a new order | `OrderDto`   | `OrderDto`       |
| PUT    | `/api/orders`           | Update an order    | `OrderDto`   | `OrderDto`       |
| PUT    | `/api/orders/{orderId}` | Update order by ID | `OrderDto`   | `OrderDto`       |
| DELETE | `/api/orders/{orderId}` | Delete order by ID | –            | `boolean`        |

### **payment-service/**

| Method | Endpoint                    | Description          | Request Body | Response Type      |
| ------ | --------------------------- | -------------------- | ------------ | ------------------ |
| GET    | `/api/payments`             | Get all payments     | –            | `List<PaymentDto>` |
| GET    | `/api/payments/{paymentId}` | Get payment by ID    | –            | `PaymentDto`       |
| POST   | `/api/payments`             | Create a new payment | `PaymentDto` | `PaymentDto`       |
| PUT    | `/api/payments`             | Update a payment     | `PaymentDto` | `PaymentDto`       |
| DELETE | `/api/payments/{paymentId}` | Delete payment by ID | –            | `boolean`          |

### **favourite-service/**

| Method | Endpoint                                          | Description                             | Request Body   | Response Type                         |
| ------ | ------------------------------------------------- | --------------------------------------- | -------------- | ------------------------------------- |
| GET    | `/api/favourites`                                 | Get all favourites                      | –              | `DtoCollectionResponse<FavouriteDto>` |
| GET    | `/api/favourites/{userId}/{productId}/{likeDate}` | Get favourite by ID (path variables)    | –              | `FavouriteDto`                        |
| GET    | `/api/favourites/find`                            | Get favourite by ID (request body)      | `FavouriteId`  | `FavouriteDto`                        |
| POST   | `/api/favourites`                                 | Create a new favourite                  | `FavouriteDto` | `FavouriteDto`                        |
| PUT    | `/api/favourites`                                 | Update a favourite                      | `FavouriteDto` | `FavouriteDto`                        |
| DELETE | `/api/favourites/{userId}/{productId}/{likeDate}` | Delete favourite by ID (path variables) | –              | `boolean`                             |
| DELETE | `/api/favourites/delete`                          | Delete favourite by ID (request body)   | `FavouriteId`  | `boolean`                             |



### **shiping-service/**

| Method | Endpoint                               | Description                   | Request Body   | Response Type        |
| ------ | -------------------------------------- | ----------------------------- | -------------- | -------------------- |
| GET    | `/api/shippings`                       | Get all order items           | –              | `List<OrderItemDto>` |
| GET    | `/api/shippings/{orderId}/{productId}` | Get order item by ID          | –              | `OrderItemDto`       |
| GET    | `/api/shippings/find`                  | Get order item by ID          | `OrderItemId`  | `OrderItemDto`       |
| POST   | `/api/shippings`                       | Create a new order item       | `OrderItemDto` | `OrderItemDto`       |
| PUT    | `/api/shippings`                       | Update an existing order item | `OrderItemDto` | `OrderItemDto`       |
| DELETE | `/api/shippings/{orderId}/{productId}` | Delete order item by ID       | –              | `boolean`            |
| DELETE | `/api/shippings/delete`                | Delete order item by ID       | `OrderItemId`  | `boolean`            |
