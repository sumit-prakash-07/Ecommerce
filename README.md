
## Low-Level Design

### User Authentication Flow

This diagram shows the login process for a user.

```mermaid
sequenceDiagram
    participant User
    participant Frontend
    participant Backend
    participant Database

    User->>Frontend: Fills and submits registration form
    Frontend->>Backend: POST /api/auth/register with user data
    activate Backend
    Backend->>Database: Check if email already exists
    Database-->>Backend: Return user status
    alt Email is unique
        Backend->>Backend: Hash the user's password using bcrypt
        Backend->>Database: Save new user to 'users' collection
        Database-->>Backend: Confirm user creation
        Backend-->>Frontend: Return success message
    else Email already exists
        Backend-->>Frontend: Return error: "User already exists"
    end
    deactivate Backend
    Frontend->>User: Display success or error message
