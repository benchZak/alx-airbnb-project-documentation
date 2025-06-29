
# 📄 Backend Requirement Specifications – Airbnb Clone

## 🔐 1. User Authentication

### ✅ Functional Requirements
- Register and log in as guest or host.
- Use JWT for secure session management.
- Support OAuth login via Google and Facebook.

### 📘 API Endpoints
- `POST /api/register`
- `POST /api/login`
- `POST /api/oauth/google`
- `GET /api/profile`
- `PUT /api/profile`

### 📥 Input Specifications
#### POST `/api/register`
```json
{
  "email": "user@example.com",
  "password": "securePass123",
  "role": "guest"
}
```

#### POST `/api/login`
```json
{
  "email": "user@example.com",
  "password": "securePass123"
}
```

### 📤 Output Example (Success)
```json
{
  "token": "jwt_token",
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "role": "host"
  }
}
```

### ❌ Validation Rules
- Valid email format, unique.
- Password ≥ 8 characters, with at least 1 number and 1 uppercase letter.
- Role must be `guest` or `host`.

### ⚙️ Performance Criteria
- Response time < 300ms.
- Brute force protection via rate limiting.

---

## 🏠 2. Property Management

### ✅ Functional Requirements
- Hosts can add, edit, and delete listings.
- Listings include title, description, location, price, amenities, and availability.

### 📘 API Endpoints
- `POST /api/properties`
- `GET /api/properties/:id`
- `PUT /api/properties/:id`
- `DELETE /api/properties/:id`

### 📥 Input Specifications
#### POST `/api/properties`
```json
{
  "title": "Beachfront Villa",
  "description": "Ocean view with 3 bedrooms",
  "location": "Agadir",
  "price": 120,
  "amenities": ["wifi", "pool", "AC"],
  "availability": ["2025-07-01", "2025-07-15"]
}
```

### 📤 Output Example
```json
{
  "id": "property-uuid",
  "status": "created"
}
```

### ❌ Validation Rules
- Title: required, max 100 chars.
- Price: positive integer.
- At least one availability date required.
- Amenities must be from predefined list.

### ⚙️ Performance Criteria
- Response < 400ms.
- Images are stored via async call (Cloudinary/S3).

---

## 📅 3. Booking System

### ✅ Functional Requirements
- Guests can book available properties.
- No double-booking allowed.
- Status: pending → confirmed → completed/canceled.

### 📘 API Endpoints
- `POST /api/bookings`
- `GET /api/bookings/:userId`
- `PUT /api/bookings/:id/cancel`
- `GET /api/bookings/status/:id`

### 📥 Input Specifications
#### POST `/api/bookings`
```json
{
  "propertyId": "uuid",
  "checkIn": "2025-07-02",
  "checkOut": "2025-07-10"
}
```

### 📤 Output Example
```json
{
  "bookingId": "uuid",
  "status": "pending"
}
```

### ❌ Validation Rules
- Valid date range (check-out > check-in).
- Dates must not overlap with existing bookings.

### ⚙️ Performance Criteria
- Availability check in < 300ms.
- Booking confirmation < 500ms.

---
