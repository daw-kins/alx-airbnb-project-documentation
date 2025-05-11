### 📄 `requirements.md`

# Backend Feature Requirement Specifications

This document outlines the technical specifications for three key backend features of the Airbnb-like platform: **User Authentication**, **Property Management**, and **Booking System**.

---

## 1. 🔐 User Authentication

### Overview

Handles user registration, login, and secure access to protected routes.

### API Endpoints

#### POST `/api/register`

 **Description:** Register a new user.

 **Input (JSON):**

  ```json
  {
    "first_name": "John",
    "last_name": "Doe",
    "email": "john@example.com",
    "password": "Secret123!",
    "phone_number": "+254712345678"
  }
  ```

 **Validation Rules:**

  * Email must be unique and valid format.
  * Password must be 8+ characters, include letters and numbers.
  * Phone must follow international format.

 **Output (Success):**

  ```json
  {
    "message": "User registered successfully",
    "user_id": "uuid"
  }
  ```

 **Error (Email Exists):**

  ```json
  {
    "error": "Email already in use"
  }
  ```

#### POST `/api/login`

 **Input:**

  ```json
  {
    "email": "john@example.com",
    "password": "Secret123!"
  }
  ```
 **Output (Success):**

  ```json
  {
    "token": "jwt_token_string"
  }
  ```
 **Error:**

  ```json
  {
    "error": "Invalid credentials"
  }
  ```

### Performance Criteria

* Response time < 500ms
* Authentication token expires in 24 hours

---

## 2. 🏡 Property Management

### Overview

Allows hosts to list, update, or delete properties.

### API Endpoints

#### POST `/api/properties`

 **Input:**

  ```json
  {
    "name": "Modern Loft",
    "description": "Great location in Nairobi",
    "location": "Nairobi",
    "price_per_night": 4500
  }
  ```

 **Auth Required:** Yes (Host only)

 **Validation:**

  * Name required.
  * Price must be > 0.

 **Output:**

  ```json
  {
    "property_id": "uuid",
    "message": "Property created"
  }
  ```

#### GET `/api/properties`

* **Description:** Fetch list of properties
* **Output:**

  ```json
  [
    {
      "property_id": "uuid",
      "name": "Modern Loft",
      "location": "Nairobi",
      "price_per_night": 4500
    }
  ]
  ```

### Performance Criteria

* Pagination supported
* Search queries < 1s

---

## 3. 📅 Booking System

### Overview

Enables users to book available properties.

### API Endpoints

#### POST `/api/bookings`

 **Input:**

  ```json
  {
    "property_id": "uuid",
    "start_date": "2025-06-01",
    "end_date": "2025-06-05"
  }
  ```

 **Auth Required:** Yes

 **Validation Rules:**

  * Start date must be before end date.
  * Property must be available for selected dates.
  * Date format: `YYYY-MM-DD`

 **Output:**

  ```json
  {
    "booking_id": "uuid",
    "total_price": 18000,
    "status": "confirmed"
  }
  ```

#### GET `/api/bookings/:user_id`

 **Description:** View bookings for a user
 **Output:**

  ```json
  [
    {
      "booking_id": "uuid",
      "property_name": "Modern Loft",
      "start_date": "2025-06-01",
      "end_date": "2025-06-05",
      "total_price": 18000
    }
  ]
  ```

### Performance Criteria

* Availability checks in < 300ms
* Concurrent bookings handled with transactions

---

## ✅ Notes

* All endpoints should follow REST conventions.
* Responses must be in JSON format.
* JWT authentication required for protected routes.
* Follow OpenAPI Specification (v3) for documentation.