## Low-Level Design

### User Authentication Flow

This diagram shows the login process for a user.

```mermaid
sequenceDiagram
    participant User
    participant Frontend
    participant Backend
    participant Database

    User->>Frontend: Fills and submits login form
    Frontend->>Backend: POST /api/auth/login with credentials
    activate Backend
    Backend->>Database: Find user by email
    Database-->>Backend: Return user data
    Backend->>Backend: Compare hashed password
    Backend->>Frontend: Return success and JWT cookie
    deactivate Backend
    Frontend->>User: Redirects to dashboard/shop

sequenceDiagram
    participant User
    participant Frontend
    participant Backend
    participant PayPal

    User->>Frontend: Clicks "Checkout"
    Frontend->>Backend: POST /api/shop/order/create with order data
    activate Backend
    Backend->>PayPal: Create payment request
    PayPal-->>Backend: Return approval URL
    Backend-->>Frontend: Send approval URL
    Frontend->>User: Redirect to PayPal for payment
    User->>PayPal: Approves payment
    PayPal->>Frontend: Redirects back with paymentId and PayerID
    Frontend->>Backend: POST /api/shop/order/capture with payment details
    activate Backend
    Backend->>Database: Update order status to "paid"
    deactivate Backend
