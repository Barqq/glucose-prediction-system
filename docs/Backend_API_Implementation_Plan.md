# Backend API Implementation Plan

## Overview

This document outlines the implementation plan for the backend API of the AI-Powered Post-Meal Glucose Prediction System. The backend will be built using **Flask** (Python) and will integrate with **Supabase** for authentication, PostgreSQL database, storage, and real-time features. The API will serve as the core interface between the frontend application and the AI models, handling user authentication, data management, and model inference.

## 1. Technology Stack

*   **Framework:** Flask (Python 3.11)
*   **Database:** Supabase (PostgreSQL)
*   **Authentication:** Supabase Auth (JWT-based)
*   **Storage:** Supabase Storage (for meal images)
*   **Real-time:** Supabase Realtime (for live glucose updates, optional)
*   **Model Serving:** Python with TensorFlow/PyTorch for model inference
*   **API Documentation:** Swagger/OpenAPI (using Flask-RESTX or similar)

## 2. Project Structure

The Flask application has been created using the `manus-create-flask-app` utility. The project structure is as follows:

```
glucose_prediction_api/
├── venv/                      # Virtual environment
├── src/
│   ├── models/                # Database models
│   │   ├── __init__.py
│   │   ├── user.py            # User model (to be modified)
│   │   ├── meal.py            # Meal model (to be created)
│   │   ├── glucose.py         # Glucose reading model (to be created)
│   │   ├── insulin.py         # Insulin log model (to be created)
│   │   └── activity.py        # Activity log model (to be created)
│   ├── routes/                # API route blueprints
│   │   ├── __init__.py
│   │   ├── user.py            # User routes (to be modified)
│   │   ├── meal.py            # Meal routes (to be created)
│   │   ├── glucose.py         # Glucose routes (to be created)
│   │   ├── insulin.py         # Insulin routes (to be created)
│   │   └── prediction.py      # Prediction routes (to be created)
│   ├── services/              # Business logic and AI model integration (to be created)
│   │   ├── __init__.py
│   │   ├── meal_analyzer.py   # Meal photo analysis service
│   │   ├── glucose_predictor.py # Glucose prediction service
│   │   └── insulin_calculator.py # Insulin dose calculation service
│   ├── utils/                 # Utility functions (to be created)
│   │   ├── __init__.py
│   │   ├── auth.py            # Authentication utilities
│   │   └── supabase_client.py # Supabase client initialization
│   ├── static/                # Static files (frontend will be placed here)
│   ├── database/
│   │   └── app.db             # SQLite database (will be replaced with Supabase)
│   └── main.py                # Main entry point
├── requirements.txt           # Python dependencies
└── README.md                  # Project documentation
```

## 3. Database Schema (Supabase PostgreSQL)

The database schema has been defined in the Data Collection and Preparation Plan. Here's a summary of the tables:

*   **users:** Stores user account information and health profile.
*   **meals:** Stores meal data including images, food labels, and nutritional estimates.
*   **simulated_cgm:** Stores simulated continuous glucose monitoring data.
*   **insulin_log:** Stores insulin dose logs.
*   **activity_log:** Stores activity, sleep, and stress data.

## 4. API Endpoints

The following API endpoints will be implemented:

### 4.1 Authentication Endpoints

*   **POST /api/auth/signup:** Register a new user.
    *   Request body: `{ "email": "string", "password": "string", "profile": {...} }`
    *   Response: `{ "user": {...}, "session": {...} }`
*   **POST /api/auth/login:** Log in an existing user.
    *   Request body: `{ "email": "string", "password": "string" }`
    *   Response: `{ "user": {...}, "session": {...} }`
*   **POST /api/auth/logout:** Log out the current user.
    *   Response: `{ "message": "Logged out successfully" }`
*   **GET /api/auth/user:** Get the current authenticated user's profile.
    *   Response: `{ "user": {...} }`

### 4.2 User Profile Endpoints

*   **GET /api/users/profile:** Get the user's health profile.
    *   Response: `{ "profile": {...} }`
*   **PUT /api/users/profile:** Update the user's health profile.
    *   Request body: `{ "age": int, "weight_kg": float, "height_cm": float, "carb_ratio": float, "correction_factor": float, ... }`
    *   Response: `{ "profile": {...} }`

### 4.3 Meal Endpoints

*   **POST /api/meals/upload:** Upload a meal photo for analysis.
    *   Request body: Multipart form data with image file.
    *   Response: `{ "meal_id": "uuid", "image_url": "string", "analysis": {...} }`
*   **GET /api/meals/:id:** Get details of a specific meal.
    *   Response: `{ "meal": {...} }`
*   **GET /api/meals:** Get a list of the user's meals (with pagination and filtering).
    *   Query parameters: `limit`, `offset`, `start_date`, `end_date`
    *   Response: `{ "meals": [...], "total": int }`
*   **DELETE /api/meals/:id:** Delete a meal record.
    *   Response: `{ "message": "Meal deleted successfully" }`

### 4.4 Glucose Endpoints

*   **POST /api/glucose:** Log a manual glucose reading.
    *   Request body: `{ "value_mg_dl": float, "timestamp": "ISO 8601 string" }`
    *   Response: `{ "glucose_reading": {...} }`
*   **GET /api/glucose:** Get the user's glucose readings (with pagination and filtering).
    *   Query parameters: `limit`, `offset`, `start_date`, `end_date`
    *   Response: `{ "readings": [...], "total": int }`

### 4.5 Insulin Endpoints

*   **POST /api/insulin:** Log an insulin dose.
    *   Request body: `{ "bolus_dose": float, "basal_dose": float, "meal_id": "uuid", "timestamp": "ISO 8601 string" }`
    *   Response: `{ "insulin_log": {...} }`
*   **GET /api/insulin:** Get the user's insulin logs (with pagination and filtering).
    *   Query parameters: `limit`, `offset`, `start_date`, `end_date`
    *   Response: `{ "logs": [...], "total": int }`

### 4.6 Prediction Endpoints

*   **POST /api/predictions/glucose:** Predict postprandial glucose curve based on a meal.
    *   Request body: `{ "meal_id": "uuid", "current_glucose_mg_dl": float }`
    *   Response: `{ "predicted_curve": [...], "peak_glucose": float, "time_to_peak": int }`
*   **POST /api/predictions/insulin:** Calculate recommended insulin dose.
    *   Request body: `{ "meal_id": "uuid", "current_glucose_mg_dl": float }`
    *   Response: `{ "recommended_dose": float, "carb_coverage": float, "correction_dose": float }`

### 4.7 Activity Endpoints

*   **POST /api/activity:** Log an activity.
    *   Request body: `{ "activity_type": "string", "duration_minutes": int, "intensity": "string", "timestamp": "ISO 8601 string" }`
    *   Response: `{ "activity_log": {...} }`
*   **GET /api/activity:** Get the user's activity logs (with pagination and filtering).
    *   Query parameters: `limit`, `offset`, `start_date`, `end_date`
    *   Response: `{ "logs": [...], "total": int }`

## 5. Supabase Integration

### 5.1 Supabase Client Initialization

A Supabase client will be initialized in `src/utils/supabase_client.py` using the `supabase-py` library.

```python
from supabase import create_client, Client
import os

SUPABASE_URL = os.environ.get("SUPABASE_URL")
SUPABASE_KEY = os.environ.get("SUPABASE_ANON_KEY")

supabase: Client = create_client(SUPABASE_URL, SUPABASE_KEY)
```

### 5.2 Authentication

Supabase Auth will be used for user authentication. The authentication utilities in `src/utils/auth.py` will handle JWT token verification and user session management.

### 5.3 Database Operations

All database operations will be performed using the Supabase client's query methods, replacing the SQLite database used in the template.

### 5.4 Storage

Meal images will be uploaded to Supabase Storage. The storage bucket will be configured to allow authenticated users to upload and retrieve their meal images.

## 6. AI Model Integration

### 6.1 Model Loading

The trained AI models (CNN, RNN/Transformer, Fusion Model) will be loaded in the respective service files (`meal_analyzer.py`, `glucose_predictor.py`, `insulin_calculator.py`) using TensorFlow or PyTorch.

### 6.2 Inference

When a prediction endpoint is called, the corresponding service will:
1.  Retrieve the necessary input data from the database.
2.  Preprocess the data (e.g., image resizing, normalization, sequence formatting).
3.  Run the model inference.
4.  Postprocess the output (e.g., denormalization, formatting).
5.  Return the prediction results.

### 6.3 Performance Optimization

*   **Model Caching:** Models will be loaded once at application startup and cached in memory.
*   **Batch Processing:** If multiple predictions are requested simultaneously, batch processing will be used to improve efficiency.
*   **Asynchronous Processing:** For long-running predictions, asynchronous task queues (e.g., Celery) may be used to avoid blocking the API.

## 7. Security Considerations

*   **HTTPS:** All API communication will be over HTTPS in production.
*   **JWT Authentication:** All protected endpoints will require a valid JWT token.
*   **Input Validation:** All user inputs will be validated to prevent injection attacks.
*   **Rate Limiting:** Rate limiting will be implemented to prevent abuse.
*   **Data Encryption:** Sensitive data will be encrypted at rest and in transit.
*   **CORS:** Cross-Origin Resource Sharing (CORS) will be configured to allow requests only from the frontend domain.

## 8. Error Handling

A consistent error handling mechanism will be implemented:

*   **Standard Error Responses:** All errors will return a JSON response with a `status`, `message`, and optional `details` field.
*   **HTTP Status Codes:** Appropriate HTTP status codes will be used (e.g., 400 for bad requests, 401 for unauthorized, 404 for not found, 500 for server errors).
*   **Logging:** All errors will be logged for debugging and monitoring purposes.

## 9. Testing

*   **Unit Tests:** Unit tests will be written for individual functions and services using `pytest`.
*   **Integration Tests:** Integration tests will verify the interaction between different components (e.g., API endpoints, database, AI models).
*   **End-to-End Tests:** End-to-end tests will simulate real user workflows to ensure the entire system functions correctly.

## 10. Deployment

The Flask application will be deployed using a platform like **Vercel**, **AWS**, **GCP**, or **Azure**. Docker will be used for containerization to ensure consistency across environments.

## 11. Next Steps

With the backend API implementation plan established, the next steps will involve:

1.  **Setting up Supabase:** Creating a Supabase project and configuring the database schema, authentication, and storage.
2.  **Implementing the database models:** Defining the SQLAlchemy models or using Supabase's query methods.
3.  **Implementing the API routes:** Creating the Flask blueprints for each endpoint.
4.  **Integrating the AI models:** Loading the trained models and implementing the inference logic.
5.  **Testing the API:** Writing and running unit, integration, and end-to-end tests.
6.  **Documenting the API:** Generating API documentation using Swagger/OpenAPI.

This comprehensive backend API implementation plan will provide a robust and scalable foundation for the AI-Powered Post-Meal Glucose Prediction System.
