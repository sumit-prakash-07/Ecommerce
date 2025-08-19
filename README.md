
## Low-Level Design

### User Authentication Flow

This diagram shows the login process for a user.

```mermaid
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
