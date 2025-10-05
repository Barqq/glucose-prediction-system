# Frontend Web Application Development Plan

## Overview

This document outlines the development plan for the frontend web application of the AI-Powered Post-Meal Glucose Prediction System. The frontend will be built using **React** with a focus on creating an **Arabic-first, right-to-left (RTL), iOS-style interface** with smooth micro-animations and a minimal, modern aesthetic. The application will integrate with the backend API to provide a seamless user experience for meal photo analysis, glucose prediction, and insulin dose calculation.

## 1. Technology Stack

*   **Framework:** React 18
*   **Build Tool:** Vite
*   **Styling:** Tailwind CSS
*   **UI Components:** shadcn/ui
*   **Icons:** Lucide Icons
*   **Charts:** Recharts
*   **Routing:** React Router DOM
*   **Animations:** Framer Motion
*   **State Management:** React Context API or Zustand (for more complex state)
*   **HTTP Client:** Axios or Fetch API
*   **Internationalization (i18n):** react-i18next

## 2. Project Structure

The React application has been created using the `manus-create-react-app` utility. The project structure will be organized as follows:

```
glucose-prediction-frontend/
├── public/                    # Public assets
├── src/
│   ├── assets/                # Static assets (images, fonts)
│   ├── components/
│   │   ├── ui/                # shadcn/ui components
│   │   ├── layout/            # Layout components (Header, Footer, Sidebar)
│   │   ├── dashboard/         # Dashboard-specific components
│   │   ├── meal/              # Meal-related components
│   │   ├── glucose/           # Glucose-related components
│   │   ├── insulin/           # Insulin-related components
│   │   └── common/            # Common/shared components
│   ├── hooks/                 # Custom React hooks
│   ├── lib/                   # Utility functions and libraries
│   ├── pages/                 # Page components (to be created)
│   │   ├── Onboarding.jsx
│   │   ├── Dashboard.jsx
│   │   ├── MealAnalyzer.jsx
│   │   ├── InsulinCalculator.jsx
│   │   ├── History.jsx
│   │   ├── Profile.jsx
│   │   └── Login.jsx
│   ├── services/              # API service functions (to be created)
│   │   ├── api.js
│   │   ├── authService.js
│   │   ├── mealService.js
│   │   ├── glucoseService.js
│   │   └── insulinService.js
│   ├── context/               # React Context providers (to be created)
│   │   └── AuthContext.jsx
│   ├── locales/               # Translation files (to be created)
│   │   ├── ar.json            # Arabic translations
│   │   └── en.json            # English translations (optional)
│   ├── App.css                # App-specific styles
│   ├── App.jsx                # Main App component
│   ├── index.css              # Global styles
│   └── main.jsx               # Entry point
├── components.json            # shadcn/ui configuration
├── eslint.config.js           # ESLint configuration
├── index.html                 # HTML entry point
├── package.json               # Project dependencies and scripts
├── pnpm-lock.yaml             # Lock file for dependencies
└── vite.config.js             # Vite bundler configuration
```

## 3. RTL (Right-to-Left) Implementation

### 3.1 Global RTL Configuration

The application will be configured to support RTL layout by default. This will be achieved by:

*   **HTML `dir` attribute:** Setting `dir="rtl"` on the `<html>` element in `index.html`.
*   **Tailwind CSS RTL Plugin:** Using the `tailwindcss-rtl` plugin or Tailwind's built-in RTL support to automatically flip layout directions.
*   **CSS Logical Properties:** Using logical properties like `margin-inline-start`, `padding-inline-end` instead of `margin-left`, `padding-right` for automatic RTL adaptation.

### 3.2 Component-Level RTL Handling

Each component will be designed with RTL in mind:

*   **Flexbox and Grid:** Using `flex-direction: row-reverse` and appropriate grid configurations for RTL layouts.
*   **Icon Flipping:** Ensuring that directional icons (arrows, navigation) are flipped appropriately using Lucide Icons' RTL variants or CSS transforms.
*   **Text Alignment:** Right-aligning text by default, with exceptions for paragraphs in non-Arabic languages.

## 4. Key Pages and Components

### 4.1 Onboarding Flow

**Pages:**
*   `WelcomeScreen.jsx`: Introduction to the app.
*   `FeaturesCarousel.jsx`: Carousel showcasing key features.
*   `HealthProfileSetup.jsx`: Form to collect user health information.
*   `PermissionsRequest.jsx`: Request camera and notification permissions.

**Components:**
*   `OnboardingLayout.jsx`: Common layout for onboarding screens.
*   `ProgressIndicator.jsx`: Visual indicator of onboarding progress.

### 4.2 Dashboard (Home Screen)

**Page:** `Dashboard.jsx`

**Components:**
*   `GreetingHeader.jsx`: Displays greeting message and current date.
*   `GlucoseChart.jsx`: Line chart showing simulated CGM data (using Recharts).
*   `QuickActionButtons.jsx`: Buttons for "Photograph Meal" and "Calculate Insulin".
*   `RecentActivityList.jsx`: List of recent meals and insulin doses.
*   `BottomNavigation.jsx`: Tab bar for navigation.

### 4.3 Meal Photo Analyzer

**Page:** `MealAnalyzer.jsx`

**Components:**
*   `CameraView.jsx`: Camera interface for capturing meal photos.
*   `ImagePreview.jsx`: Preview of captured image with retake/analyze options.
*   `AnalysisResults.jsx`: Display of meal identification, nutritional breakdown, and predicted glucose curve.
*   `LoadingIndicator.jsx`: Loading animation during analysis.

### 4.4 Insulin Calculator

**Page:** `InsulinCalculator.jsx`

**Components:**
*   `InputForm.jsx`: Form for current glucose, carbohydrates, and target glucose.
*   `CalculationDisplay.jsx`: Step-by-step display of insulin dose calculation.
*   `DoseConfirmation.jsx`: Confirmation screen with recommended dose.

### 4.5 Alert Screen (Early Warning)

**Component:** `AlertBanner.jsx` (can be used across pages)

**Features:**
*   Displays alerts for predicted hypo/hyperglycemia.
*   Color-coded severity indicators.
*   Actionable recommendations.

### 4.6 History Screen

**Page:** `History.jsx`

**Components:**
*   `FilterOptions.jsx`: Date range and meal type filters.
*   `TimelineView.jsx`: Chronological list of entries.
*   `EntryCard.jsx`: Compact card for each meal/insulin log entry.

### 4.7 Profile Screen

**Page:** `Profile.jsx`

**Components:**
*   `UserInfo.jsx`: Display of user information.
*   `HealthSettings.jsx`: Editable health settings (CR, CF, target glucose).
*   `AppSettings.jsx`: Language, notifications, data sync settings.
*   `SupportLinks.jsx`: Links to help center, privacy policy, terms of service.

### 4.8 Login/Signup

**Pages:**
*   `Login.jsx`: Login form.
*   `Signup.jsx`: Signup form with health profile setup.

**Components:**
*   `AuthForm.jsx`: Reusable form component for authentication.

## 5. API Integration

### 5.1 API Service Layer

A dedicated service layer will be created to handle all API requests. This will abstract the API calls from the components, making the code more maintainable and testable.

**Example (`src/services/api.js`):**

```javascript
import axios from 'axios';

const API_BASE_URL = import.meta.env.VITE_API_BASE_URL || '/api';

const api = axios.create({
  baseURL: API_BASE_URL,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Add JWT token to requests
api.interceptors.request.use((config) => {
  const token = localStorage.getItem('authToken');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

export default api;
```

**Example (`src/services/mealService.js`):**

```javascript
import api from './api';

export const uploadMealPhoto = async (imageFile) => {
  const formData = new FormData();
  formData.append('image', imageFile);
  const response = await api.post('/meals/upload', formData, {
    headers: {
      'Content-Type': 'multipart/form-data',
    },
  });
  return response.data;
};

export const getMealDetails = async (mealId) => {
  const response = await api.get(`/meals/${mealId}`);
  return response.data;
};

export const getUserMeals = async (params) => {
  const response = await api.get('/meals', { params });
  return response.data;
};
```

### 5.2 Authentication Context

An `AuthContext` will be created to manage user authentication state across the application.

**Example (`src/context/AuthContext.jsx`):**

```javascript
import React, { createContext, useState, useEffect } from 'react';
import { login as apiLogin, signup as apiSignup, logout as apiLogout, getCurrentUser } from '../services/authService';

export const AuthContext = createContext();

export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Check if user is already logged in
    const checkAuth = async () => {
      try {
        const userData = await getCurrentUser();
        setUser(userData);
      } catch (error) {
        console.error('Not authenticated');
      } finally {
        setLoading(false);
      }
    };
    checkAuth();
  }, []);

  const login = async (email, password) => {
    const { user, session } = await apiLogin(email, password);
    setUser(user);
    localStorage.setItem('authToken', session.access_token);
  };

  const signup = async (email, password, profile) => {
    const { user, session } = await apiSignup(email, password, profile);
    setUser(user);
    localStorage.setItem('authToken', session.access_token);
  };

  const logout = async () => {
    await apiLogout();
    setUser(null);
    localStorage.removeItem('authToken');
  };

  return (
    <AuthContext.Provider value={{ user, loading, login, signup, logout }}>
      {children}
    </AuthContext.Provider>
  );
};
```

## 6. Internationalization (i18n)

### 6.1 Setup

`react-i18next` will be used for internationalization. Translation files will be created for Arabic (and optionally English).

**Installation:**

```bash
pnpm add react-i18next i18next
```

**Configuration (`src/lib/i18n.js`):**

```javascript
import i18n from 'i18next';
import { initReactI18next } from 'react-i18next';
import ar from '../locales/ar.json';
import en from '../locales/en.json';

i18n
  .use(initReactI18next)
  .init({
    resources: {
      ar: { translation: ar },
      en: { translation: en },
    },
    lng: 'ar', // Default language
    fallbackLng: 'ar',
    interpolation: {
      escapeValue: false,
    },
  });

export default i18n;
```

### 6.2 Translation Files

**Example (`src/locales/ar.json`):**

```json
{
  "welcome": "مرحبًا",
  "dashboard": "لوحة المعلومات",
  "meal_analyzer": "محلل الوجبات",
  "insulin_calculator": "حاسبة الأنسولين",
  "history": "السجل",
  "profile": "الملف الشخصي",
  "photograph_meal": "تصوير الوجبة",
  "calculate_insulin": "حساب الأنسولين",
  "current_glucose": "الجلوكوز الحالي",
  "carbohydrates": "الكربوهيدرات",
  "recommended_dose": "الجرعة الموصى بها",
  "confirm": "تأكيد",
  "cancel": "إلغاء"
}
```

### 6.3 Usage in Components

```javascript
import { useTranslation } from 'react-i18next';

function Dashboard() {
  const { t } = useTranslation();

  return (
    <div>
      <h1>{t('dashboard')}</h1>
      <button>{t('photograph_meal')}</button>
    </div>
  );
}
```

## 7. Micro-Animations with Framer Motion

Framer Motion will be used to add smooth micro-animations throughout the application.

**Example (Fade-in animation):**

```javascript
import { motion } from 'framer-motion';

function Card({ children }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.3 }}
      className="card"
    >
      {children}
    </motion.div>
  );
}
```

**Example (Button press animation):**

```javascript
import { motion } from 'framer-motion';

function Button({ children, onClick }) {
  return (
    <motion.button
      whileTap={{ scale: 0.95 }}
      onClick={onClick}
      className="button"
    >
      {children}
    </motion.button>
  );
}
```

## 8. Styling with Tailwind CSS

Tailwind CSS will be used for all styling, with a focus on creating an iOS-style interface.

**Key Tailwind Utilities:**
*   **Rounded Corners:** `rounded-xl` (12px radius for cards).
*   **Shadows:** `shadow-md` for depth.
*   **Colors:** Using the color palette defined in `App.css` (primary, secondary, success, warning, error).
*   **Typography:** Using SF Arabic font family (to be added via Google Fonts or local fonts).
*   **Spacing:** Consistent use of spacing utilities (`p-4`, `m-2`, etc.).

## 9. Responsive Design

The application will be optimized for various screen sizes:

*   **Mobile-first approach:** Designing for mobile screens first, then adapting for larger screens.
*   **Breakpoints:** Using Tailwind's responsive breakpoints (`sm:`, `md:`, `lg:`, `xl:`).
*   **Flexible layouts:** Using Flexbox and Grid for responsive layouts.

## 10. Testing

*   **Unit Tests:** Testing individual components using React Testing Library.
*   **Integration Tests:** Testing the interaction between components and API services.
*   **End-to-End Tests:** Using tools like Cypress or Playwright to test user workflows.

## 11. Deployment

The React application will be built for production using Vite's build command (`pnpm run build`). The built files will be placed in the `dist` directory and can be deployed to various platforms:

*   **Vercel:** For easy deployment of React applications.
*   **Netlify:** Another popular platform for static site hosting.
*   **Integration with Backend:** The built frontend can be served from the Flask backend's static directory for a unified deployment.

## 12. Next Steps

With the frontend web application development plan established, the next steps will involve:

1.  **Setting up i18n and translation files.**
2.  **Implementing the page components and layouts.**
3.  **Creating reusable UI components.**
4.  **Integrating with the backend API.**
5.  **Adding RTL support and testing.**
6.  **Implementing micro-animations.**
7.  **Testing the application across different devices and browsers.**

This comprehensive frontend web application development plan will ensure a high-quality, user-friendly, and culturally appropriate interface for the AI-Powered Post-Meal Glucose Prediction System.
