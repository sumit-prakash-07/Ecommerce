
## Low-Level Design

### User Authentication Flow

This diagram shows the login process for a user.

```mermaid
sequenceDiagram
    participant Admin
    participant Frontend
    participant Cloudinary
    participant Backend
    participant Database

    Admin->>Frontend: Fills out "Add Product" form and selects image
    Frontend->>Cloudinary: Uploads image file
    activate Cloudinary
    Cloudinary-->>Frontend: Returns secure image URL
    deactivate Cloudinary
    
    Frontend->>Backend: POST /api/admin/products/add with product data and image URL
    activate Backend
    Backend->>Database: Insert new document into 'products' collection
    Database-->>Backend: Confirm new product saved
    Backend-->>Frontend: Return success message and new product data
    deactivate Backend
    
    Frontend->>Admin: Displays "Product added successfully"
