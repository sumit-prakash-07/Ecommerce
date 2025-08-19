
## Low-Level Design

### User Authentication Flow

This diagram shows the login process for a user.

```mermaid
sequenceDiagram
    participant Customer
    participant Frontend
    participant Backend
    participant Database

    Customer->>Frontend: Writes and submits a review for a product
    Frontend->>Backend: POST /api/shop/review/add with product ID, user ID, and review
    activate Backend
    Backend->>Database: Query 'orders' to verify user purchased the product
    Database-->>Backend: Return order status
    
    alt User has purchased the product
        Backend->>Database: Insert new review into 'reviews' collection
        Database-->>Backend: Confirm review saved
        Backend->>Backend: Recalculate average review score for the product
        Backend->>Database: Update 'products' collection with new average score
        Database-->>Backend: Confirm product update
        Backend-->>Frontend: Return success message
    else User has not purchased the product
        Backend-->>Frontend: Return error: "You must purchase this product to review it"
    end
    deactivate Backend

    Frontend->>Customer: Display confirmation or error message
