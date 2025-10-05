# API Documentation: AI-Powered Post-Meal Glucose Prediction System

## Overview

This document provides comprehensive API documentation for the AI-Powered Post-Meal Glucose Prediction System. The API serves as the central communication hub for the frontend application, enabling user authentication, data management, and interaction with the AI models for meal analysis, glucose prediction, and insulin dose calculation. The API is built using Flask and integrates with Supabase for backend services.

## 1. Base URL

The base URL for all API endpoints will be `https://your-api-domain.com/api` (replace `your-api-domain.com` with the actual deployed domain).

## 2. Authentication

All protected endpoints require a JSON Web Token (JWT) for authentication. The JWT should be included in the `Authorization` header of each request as a Bearer token.

**Example:**

```
Authorization: Bearer <your_jwt_token>
```

## 3. Error Handling

Errors are returned in a consistent JSON format with appropriate HTTP status codes.

**Example Error Response:**

```json
{
  "status": "error",
  "message": "Invalid credentials",
  "details": "The email or password provided is incorrect."
}
```

## 4. Endpoints

### 4.1 Authentication Endpoints

#### `POST /api/auth/signup`

Registers a new user and creates a new account.

*   **Request Body:**
    ```json
    {
      "email": "string",
      "password": "string",
      "profile": {
        "age": "integer",
        "weight_kg": "float",
        "height_cm": "float",
        "diabetes_type": "string",
        "carb_ratio": "float",
        "correction_factor": "float",
        "target_glucose_mg_dl": "integer"
      }
    }
    ```
*   **Response (Success 201 Created):**
    ```json
    {
      "status": "success",
      "message": "User registered successfully",
      "user": {
        "id": "uuid",
        "email": "string",
        "profile": {...}
      },
      "session": {
        "access_token": "string",
        "expires_at": "timestamp"
      }
    }
    ```
*   **Response (Error 400 Bad Request):** Email already exists or invalid profile data.

#### `POST /api/auth/login`

Authenticates a user and returns a JWT.

*   **Request Body:**
    ```json
    {
      "email": "string",
      "password": "string"
    }
    ```
*   **Response (Success 200 OK):**
    ```json
    {
      "status": "success",
      "message": "Logged in successfully",
      "user": {
        "id": "uuid",
        "email": "string",
        "profile": {...}
      },
      "session": {
        "access_token": "string",
        "expires_at": "timestamp"
      }
    }
    ```
*   **Response (Error 401 Unauthorized):** Invalid credentials.

#### `POST /api/auth/logout`

Logs out the current user by invalidating their session.

*   **Authentication:** Required (JWT).
*   **Response (Success 200 OK):**
    ```json
    {
      "status": "success",
      "message": "Logged out successfully"
    }
    ```

#### `GET /api/auth/user`

Retrieves the profile of the currently authenticated user.

*   **Authentication:** Required (JWT).
*   **Response (Success 200 OK):**
    ```json
    {
      "status": "success",
      "user": {
        "id": "uuid",
        "email": "string",
        "profile": {...}
      }
    }
    ```
*   **Response (Error 401 Unauthorized):** Invalid or missing token.

### 4.2 User Profile Endpoints

#### `GET /api/users/profile`

Retrieves the health profile of the authenticated user.

*   **Authentication:** Required (JWT).
*   **Response (Success 200 OK):**
    ```json
    {
      "status": "success",
      "profile": {
        "age": "integer",
        "weight_kg": "float",
        "height_cm": "float",
        "diabetes_type": "string",
        "carb_ratio": "float",
        "correction_factor": "float",
        "target_glucose_mg_dl": "integer",
        "created_at": "timestamp"
      }
    }
    ```

#### `PUT /api/users/profile`

Updates the health profile of the authenticated user.

*   **Authentication:** Required (JWT).
*   **Request Body:**
    ```json
    {
      "age": "integer" (optional),
      "weight_kg": "float" (optional),
      "height_cm": "float" (optional),
      "diabetes_type": "string" (optional),
      "carb_ratio": "float" (optional),
      "correction_factor": "float" (optional),
      "target_glucose_mg_dl": "integer" (optional)
    }
    ```
*   **Response (Success 200 OK):**
    ```json
    {
      "status": "success",
      "message": "Profile updated successfully",
      "profile": {...} // Updated profile data
    }
    ```

### 4.3 Meal Endpoints

#### `POST /api/meals/upload`

Uploads a meal photo for AI analysis and logs the meal.

*   **Authentication:** Required (JWT).
*   **Request Body:** Multipart form data with an image file.
    *   `image`: The meal photo file.
*   **Response (Success 201 Created):**
    ```json
    {
      "status": "success",
      "message": "Meal uploaded and analyzed successfully",
      "meal_id": "uuid",
      "image_url": "string",
      "analysis": {
        "food_label": "string",
        "portion_size": "string",
        "estimated_carbs_g": "float",
        "estimated_protein_g": "float",
        "estimated_fat_g": "float",
        "estimated_calories": "float"
      }
    }
    ```

#### `GET /api/meals/:id`

Retrieves details of a specific meal by its ID.

*   **Authentication:** Required (JWT).
*   **Path Parameters:**
    *   `id`: UUID of the meal.
*   **Response (Success 200 OK):**
    ```json
    {
      "status": "success",
      "meal": {
        "id": "uuid",
        "user_id": "uuid",
        "image_path": "string",
        "food_label": "string",
        "portion_size": "string",
        "estimated_carbs_g": "float",
        "estimated_protein_g": "float",
        "estimated_fat_g": "float",
        "estimated_calories": "float",
        "timestamp": "timestamp"
      }
    }
    ```
*   **Response (Error 404 Not Found):** Meal not found or not owned by user.

#### `GET /api/meals`

Retrieves a list of the authenticated user's meals.

*   **Authentication:** Required (JWT).
*   **Query Parameters (Optional):**
    *   `limit`: Integer, number of results to return (default: 10).
    *   `offset`: Integer, number of results to skip (default: 0).
    *   `start_date`: ISO 8601 string, filter meals after this date.
    *   `end_date`: ISO 8601 string, filter meals before this date.
*   **Response (Success 200 OK):**
    ```json
    {
      "status": "success",
      "meals": [
        {...}, // Meal object
        {...}
      ],
      "total": "integer"
    }
    ```

#### `DELETE /api/meals/:id`

Deletes a specific meal record.

*   **Authentication:** Required (JWT).
*   **Path Parameters:**
    *   `id`: UUID of the meal.
*   **Response (Success 200 OK):**
    ```json
    {
      "status": "success",
      "message": "Meal deleted successfully"
    }
    ```
*   **Response (Error 404 Not Found):** Meal not found or not owned by user.

### 4.4 Glucose Endpoints

#### `POST /api/glucose`

Logs a manual glucose reading for the authenticated user.

*   **Authentication:** Required (JWT).
*   **Request Body:**
    ```json
    {
      "value_mg_dl": "float",
      "timestamp": "ISO 8601 string" (optional, defaults to now)
    }
    ```
*   **Response (Success 201 Created):**
    ```json
    {
      "status": "success",
      "message": "Glucose reading logged successfully",
      "glucose_reading": {
        "id": "uuid",
        "user_id": "uuid",
        "value_mg_dl": "float",
        "timestamp": "timestamp"
      }
    }
    ```

#### `GET /api/glucose`

Retrieves a list of the authenticated user's glucose readings.

*   **Authentication:** Required (JWT).
*   **Query Parameters (Optional):**
    *   `limit`: Integer, number of results to return (default: 10).
    *   `offset`: Integer, number of results to skip (default: 0).
    *   `start_date`: ISO 8601 string, filter readings after this date.
    *   `end_date`: ISO 8601 string, filter readings before this date.
*   **Response (Success 200 OK):**
    ```json
    {
      "status": "success",
      "readings": [
        {...}, // Glucose reading object
        {...}
      ],
      "total": "integer"
    }
    ```

### 4.5 Insulin Endpoints

#### `POST /api/insulin`

Logs an insulin dose for the authenticated user.

*   **Authentication:** Required (JWT).
*   **Request Body:**
    ```json
    {
      "bolus_dose": "float",
      "basal_dose": "float" (optional),
      "meal_id": "uuid" (optional, link to a meal),
      "timestamp": "ISO 8601 string" (optional, defaults to now)
    }
    ```
*   **Response (Success 201 Created):**
    ```json
    {
      "status": "success",
      "message": "Insulin dose logged successfully",
      "insulin_log": {
        "id": "uuid",
        "user_id": "uuid",
        "bolus_dose": "float",
        "basal_dose": "float" (nullable),
        "meal_id": "uuid" (nullable),
        "timestamp": "timestamp"
      }
    }
    ```

#### `GET /api/insulin`

Retrieves a list of the authenticated user's insulin logs.

*   **Authentication:** Required (JWT).
*   **Query Parameters (Optional):**
    *   `limit`: Integer, number of results to return (default: 10).
    *   `offset`: Integer, number of results to skip (default: 0).
    *   `start_date`: ISO 8601 string, filter logs after this date.
    *   `end_date`: ISO 8601 string, filter logs before this date.
*   **Response (Success 200 OK):**
    ```json
    {
      "status": "success",
      "logs": [
        {...}, // Insulin log object
        {...}
      ],
      "total": "integer"
    }
    ```

### 4.6 Prediction Endpoints

#### `POST /api/predictions/glucose`

Predicts the postprandial glucose curve based on a meal and current glucose level.

*   **Authentication:** Required (JWT).
*   **Request Body:**
    ```json
    {
      "meal_id": "uuid",
      "current_glucose_mg_dl": "float"
    }
    ```
*   **Response (Success 200 OK):**
    ```json
    {
      "status": "success",
      "message": "Glucose prediction successful",
      "predicted_curve": [
        { "timestamp": "ISO 8601 string", "glucose_mg_dl": "float" },
        { "timestamp": "ISO 8601 string", "glucose_mg_dl": "float" }
      ], // Time-series of predicted glucose values
      "peak_glucose_mg_dl": "float",
      "time_to_peak_minutes": "integer"
    }
    ```

#### `POST /api/predictions/insulin`

Calculates the recommended bolus insulin dose based on a meal and current glucose level.

*   **Authentication:** Required (JWT).
*   **Request Body:**
    ```json
    {
      "meal_id": "uuid",
      "current_glucose_mg_dl": "float"
    }
    ```
*   **Response (Success 200 OK):**
    ```json
    {
      "status": "success",
      "message": "Insulin dose calculation successful",
      "recommended_dose_units": "float",
      "carb_coverage_dose_units": "float",
      "correction_dose_units": "float"
    }
    ```

### 4.7 Activity Endpoints

#### `POST /api/activity`

Logs an activity for the authenticated user.

*   **Authentication:** Required (JWT).
*   **Request Body:**
    ```json
    {
      "activity_type": "string",
      "duration_minutes": "integer",
      "intensity": "string" (e.g., "low", "medium", "high"),
      "timestamp": "ISO 8601 string" (optional, defaults to now)
    }
    ```
*   **Response (Success 201 Created):**
    ```json
    {
      "status": "success",
      "message": "Activity logged successfully",
      "activity_log": {
        "id": "uuid",
        "user_id": "uuid",
        "activity_type": "string",
        "duration_minutes": "integer",
        "intensity": "string",
        "timestamp": "timestamp"
      }
    }
    ```

#### `GET /api/activity`

Retrieves a list of the authenticated user's activity logs.

*   **Authentication:** Required (JWT).
*   **Query Parameters (Optional):**
    *   `limit`: Integer, number of results to return (default: 10).
    *   `offset`: Integer, number of results to skip (default: 0).
    *   `start_date`: ISO 8601 string, filter logs after this date.
    *   `end_date`: ISO 8601 string, filter logs before this date.
*   **Response (Success 200 OK):**
    ```json
    {
      "status": "success",
      "logs": [
        {...}, // Activity log object
        {...}
      ],
      "total": "integer"
    }
    ```

