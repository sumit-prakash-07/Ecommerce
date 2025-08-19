
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
