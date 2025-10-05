# AI-Powered Post-Meal Glucose Prediction System

## Overview

This project develops an **AI-Powered Post-Meal Glucose Prediction System** designed to assist individuals with diabetes in managing their blood glucose levels more effectively. The system features an **Arabic-first, iOS-style web application** that leverages advanced Artificial Intelligence to analyze meal photos, predict post-meal glucose excursions, and recommend precise insulin doses. Targeting the Saudi Arabian market, the system aims for 100% AI-based calculation accuracy for insulin doses, providing a personalized and culturally relevant solution for diabetes management.

## Features

*   **Arabic-First, iOS-Style Interface:** Intuitive and culturally appropriate user experience with full Right-to-Left (RTL) support, minimal design, and smooth micro-animations.
*   **Meal Photo Analyzer:** Utilizes Convolutional Neural Networks (CNNs) to identify food items from photos, estimate portion sizes, and calculate macronutrient content (especially carbohydrates) of Saudi cuisine.
*   **AI-Powered Glucose Prediction:** Employs Recurrent Neural Networks (RNNs) and Transformer models to predict post-meal blood glucose trends based on meal intake, historical glucose data, insulin doses, and activity levels.
*   **100% AI-Based Insulin Dose Calculation:** A sophisticated Fusion Model integrates meal analysis and glucose prediction to provide highly accurate bolus insulin dose recommendations.
*   **Early Warning Engine:** Proactively alerts users to potential hypoglycemic or hyperglycemic events, enabling timely intervention.
*   **Personalized Health Profile:** Allows users to manage their diabetes-related parameters (Carb Ratio, Correction Factor, Target Glucose).
*   **Comprehensive History Tracking:** Logs and visualizes past meals, glucose readings, insulin doses, and activities.
*   **Secure and Scalable Backend:** Built with Flask and Supabase for robust data management, authentication, and real-time capabilities.

## Technology Stack

### Frontend

*   **Framework:** React 18
*   **Build Tool:** Vite
*   **Styling:** Tailwind CSS, shadcn/ui
*   **Icons:** Lucide Icons
*   **Charts:** Recharts
*   **Animations:** Framer Motion
*   **State Management:** React Context API
*   **Internationalization:** react-i18next

### Backend

*   **Framework:** Flask (Python 3.11)
*   **Database:** Supabase (PostgreSQL)
*   **Authentication:** Supabase Auth (JWT-based)
*   **Storage:** Supabase Storage (for meal images)
*   **AI Model Serving:** TensorFlow/PyTorch

### AI Models

*   **Meal Photo Analyzer:** ResNet or EfficientNet (CNNs)
*   **Glucose Prediction:** LSTM or Transformer Networks (RNNs)
*   **Insulin Dose Calculation:** Custom Fusion Model

### Deployment & CI/CD

*   **Containerization:** Docker
*   **CI/CD:** GitHub Actions
*   **Hosting:** Vercel (recommended)

## Installation and Setup

To set up the project locally, follow these steps:

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/glucose-prediction-system.git
cd glucose-prediction-system
```

### 2. Supabase Setup

1.  Go to [Supabase](https://supabase.com/) and create a new project.
2.  Note down your Project URL and `anon` key.
3.  Set up your database schema as defined in `Data_Collection_and_Preparation_Plan.md`.
4.  Configure Storage buckets for meal images.

### 3. Backend Setup

1.  Navigate to the backend directory:
    ```bash
    cd glucose_prediction_api
    ```
2.  Create a Python virtual environment and activate it:
    ```bash
    python3.11 -m venv venv
    source venv/bin/activate
    ```
3.  Install Python dependencies:
    ```bash
    pip install -r requirements.txt
    ```
4.  Create a `.env` file in the `glucose_prediction_api` directory and add your Supabase credentials:
    ```
    SUPABASE_URL="YOUR_SUPABASE_URL"
    SUPABASE_ANON_KEY="YOUR_SUPABASE_ANON_KEY"
    FLASK_SECRET_KEY="a_strong_random_secret_key"
    ```
5.  (Optional) Place your trained AI models in a designated directory (e.g., `src/models/trained_models/`).

### 4. Frontend Setup

1.  Navigate to the frontend directory:
    ```bash
    cd ../glucose-prediction-frontend
    ```
2.  Install Node.js dependencies using pnpm:
    ```bash
    pnpm install
    ```
3.  Create a `.env` file in the `glucose-prediction-frontend` directory and add your API base URL:
    ```
    VITE_API_BASE_URL="http://localhost:5000/api" # Or your deployed backend URL
    ```

## Usage

### 1. Start the Backend Server

From the `glucose_prediction_api` directory:

```bash
source venv/bin/activate
python src/main.py
```

### 2. Start the Frontend Development Server

From the `glucose-prediction-frontend` directory:

```bash
pnpm run dev
```

Open your browser and navigate to `http://localhost:5173` (or the port indicated by Vite) to access the application.

## Contributing

We welcome contributions to this project! Please follow these steps:

1.  Fork the repository.
2.  Create a new branch (`git checkout -b feature/your-feature-name`).
3.  Make your changes and commit them (`git commit -m 'Add new feature'`).
4.  Push to the branch (`git push origin feature/your-feature-name`).
5.  Open a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contact

For any inquiries or support, please open an issue in the GitHub repository or contact [your-email@example.com].

---

**Note:** This project is for educational and research purposes only and should not be used for actual medical diagnosis or treatment without proper clinical validation and regulatory approval. Always consult with a qualified healthcare professional for medical advice.
