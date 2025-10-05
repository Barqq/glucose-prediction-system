# Technical Appendix: AI-Powered Post-Meal Glucose Prediction System

## Overview

This Technical Appendix provides an in-depth description of the Artificial Intelligence (AI) models, data handling, and overall system architecture for the AI-Powered Post-Meal Glucose Prediction System. It details the methodologies employed for meal photo analysis, post-meal glucose prediction, and 100% AI-based insulin dose calculation, along with the evaluation and validation strategies.

## 1. Data Collection and Preparation

As outlined in the [Data Collection and Preparation Plan](/home/ubuntu/Data_Collection_and_Preparation_Plan.md), a multi-pronged approach is adopted to overcome the limitations of existing datasets, particularly the scarcity of Saudi-specific food data and postprandial glucose information.

### 1.1 Data Sources and Augmentation

*   **Saudi Branded Food Database (SBFD):** While primarily focused on pre-packed items, the SBFD serves as a foundational reference for nutritional content of some commercially available products in Saudi Arabia [1].
*   **Public Food Image and Nutrition Datasets:** Datasets like Food-101 [2], Nutrition5k [3], and specialized Middle Eastern food datasets [4, 5] provide a basis for food recognition and nutritional estimation, though they require augmentation for Saudi cuisine specificity.
*   **Meal Photo Collection Protocol:** A standardized protocol is developed for collecting high-quality meal photos, including multiple portion sizes and detailed annotations (food labels, ingredients).
*   **Simulated Postprandial Glucose Data:** To address the lack of real-world postprandial glucose data, a physiological model simulates glucose responses based on estimated macronutrient content, user-specific parameters (e.g., insulin sensitivity), and established physiological principles. This generates paired datasets of (image, nutritional info, glucose curve).
*   **Synthetic Data Augmentation:** Techniques such as image transformations (rotation, scaling, cropping) and Generative Adversarial Networks (GANs) are employed to increase dataset size and diversity.

### 1.2 Database Schema

A PostgreSQL database, managed via Supabase, stores all collected and generated data. The schema includes tables for `users`, `meals`, `simulated_cgm`, `insulin_log`, and `activity_log`, ensuring a structured approach to data management. Detailed schema is available in the [Data Collection and Preparation Plan](/home/ubuntu/Data_Collection_and_Preparation_Plan.md).

## 2. AI Model Architectures and Training

The system employs a modular AI architecture comprising a Convolutional Neural Network (CNN) for meal analysis, a Recurrent Neural Network (RNN) or Transformer for glucose prediction, and a Fusion Model for integrated insulin dose calculation.

### 2.1 Meal Photo Analyzer: Food Recognition and Carbohydrate Estimation (CNN)

*   **Objective:** Identify food items and estimate macronutrient content from meal images.
*   **Architecture:** State-of-the-art CNNs such as **ResNet** (e.g., ResNet-50, ResNet-101) or **EfficientNet** are utilized as backbone architectures. These models are pre-trained on large image datasets (e.g., ImageNet) and fine-tuned on our specialized food image dataset.
*   **Input:** RGB meal image.
*   **Outputs:**
    *   **Food Label:** Categorical classification (e.g., "Kabsa", "Mandi").
    *   **Portion Size:** Regression output (e.g., grams, or categorical: small/medium/large).
    *   **Macronutrient Estimation:** Regression outputs for carbohydrates (g), proteins (g), and fats (g).
*   **Loss Functions:** Cross-Entropy Loss for classification and Mean Squared Error (MSE) or Mean Absolute Error (MAE) for regression tasks.
*   **Equation for Carbohydrate Estimation:**
    $$ \text{Carbs} = f_{\text{CNN}}(\text{Image}) $$
    Where $f_{\text{CNN}}$ is the CNN model mapping the input image to estimated carbohydrate content.

### 2.2 Post-Meal Glucose Prediction (RNN/Transformer)

*   **Objective:** Predict the time-series of postprandial blood glucose levels.
*   **Architecture:** **Long Short-Term Memory (LSTM) Networks** or **Transformer Networks** are employed due to their efficacy in handling sequential data and capturing long-term dependencies. Transformers, with their self-attention mechanisms, are particularly effective for complex temporal relationships.
*   **Input:** A sequence of historical data points including previous glucose readings, meal macronutrients and timestamps, insulin doses and timestamps, and activity levels.
*   **Output:** Predicted glucose values (mg/dL) at future time steps (e.g., every 15 minutes for 2-4 hours post-meal).
*   **Loss Function:** MSE or MAE for regression.
*   **Equation for Glucose Prediction:**
    $$ \text{Glucose}_{t+1} = f_{\text{RNN/Transformer}}(\text{Glucose}_t, \text{Meal}_t, \text{Insulin}_t, \text{Activity}_t, \dots) $$
    Where $f_{\text{RNN/Transformer}}$ is the model predicting the next glucose value based on current and historical inputs.

### 2.3 Fusion Model: Integrated Prediction and Insulin Dose Calculation

*   **Objective:** Combine outputs from the CNN and RNN/Transformer models to provide a comprehensive glucose curve and calculate recommended insulin doses.
*   **Architecture:** A neural network (e.g., Multi-Layer Perceptron) that takes the estimated carbohydrates from the CNN, the predicted glucose curve from the RNN/Transformer, user-specific parameters (Carb Ratio, Correction Factor), and current glucose level as inputs.
*   **Outputs:**
    *   **Refined Postprandial Glucose Curve:** An integrated time-series prediction.
    *   **Recommended Insulin Dose:** A precise bolus insulin dose.
*   **Insulin Dose Calculation (100% AI-based):** The standard insulin dose formula is used, with all parameters derived or refined by the AI models.
    $$ \text{Insulin Dose} = \left( \frac{\text{Estimated Carbs}}{\text{Carb Ratio}} \right) + \left( \frac{\text{Current Glucose} - \text{Target Glucose}}{\text{Correction Factor}} \right) $$
    Here, `Estimated Carbs` is from the CNN, `Current Glucose` is the latest reading (potentially refined by RNN/Transformer), and `Carb Ratio` and `Correction Factor` are user-specific, potentially dynamically adjusted by AI.
*   **Loss Function:** A multi-task loss combining MSE/MAE for glucose prediction and a custom loss for insulin dose accuracy, potentially incorporating medical constraints.

### 2.4 Training Methodology

*   **Data Splitting:** Datasets are split into training, validation, and test sets (e.g., 70/15/15%).
*   **Hyperparameter Tuning:** Techniques like grid search, random search, or Bayesian optimization are used.
*   **Optimization:** Adam optimizer with dynamic learning rate schedules (e.g., learning rate decay).
*   **Early Stopping:** Implemented based on validation loss to prevent overfitting.

## 3. Evaluation Metrics and Validation

Rigorous evaluation is critical for a medical device. Metrics are chosen to reflect both predictive accuracy and clinical relevance.

### 3.1 For Glucose Prediction (Regression Models)

*   **Mean Absolute Error (MAE):** $ \text{MAE} = \frac{1}{N} \sum_{i=1}^{N} |y_i - \hat{y}_i| $
*   **Root Mean Squared Error (RMSE):** $ \text{RMSE} = \sqrt{\frac{1}{N} \sum_{i=1}^{N} (y_i - \hat{y}_i)^2 } $
*   **Mean Absolute Relative Difference (MARD):** $ \text{MARD} = \frac{1}{N} \sum_{i=1}^{N} \frac{|y_i - \hat{y}_i|}{y_i} \times 100\% $. This metric is crucial for comparing against simulated clinical CGM accuracy standards.

### 3.2 For Hypo/Hyperglycemia Prediction (Classification)

*   **Sensitivity (Recall):** $ \text{Sensitivity} = \frac{\text{True Positives}}{\text{True Positives} + \text{False Negatives}} $
*   **Specificity:** $ \text{Specificity} = \frac{\text{True Negatives}}{\text{True Negatives} + \text{False Positives}} $
*   **Precision:** $ \text{Precision} = \frac{\text{True Positives}}{\text{True Positives} + \text{False Positives}} $
*   **F1-Score:** $ \text{F1-Score} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}} $

### 3.3 Cross-Validation and Baseline Comparison

*   **K-Fold Cross-Validation:** Ensures model generalization and robustness.
*   **Baseline Models:** Performance is benchmarked against simpler models to demonstrate AI value.

### 3.4 Confidence Intervals and Uncertainty Estimates

Confidence intervals and uncertainty estimates are provided for all predictions to inform clinical decision-making and meet regulatory requirements.

## 4. Verification and Validation

*   **Test Dataset Validation:** Models are rigorously tested on independent datasets.
*   **Simulated Clinical Accuracy:** Performance is compared against simulated clinical CGM accuracy standards.
*   **100% AI-based Computation:** Strict adherence to the project requirement for AI-driven insulin dose calculation.

## 5. Backend API and Frontend Integration

### 5.1 Backend API (Flask)

*   **Framework:** Flask (Python).
*   **Database:** Supabase (PostgreSQL) for data persistence.
*   **Authentication:** Supabase Auth for user management and JWT-based authentication.
*   **Storage:** Supabase Storage for meal images.
*   **Endpoints:** Comprehensive set of RESTful API endpoints for user management, meal data, glucose logs, insulin logs, activity logs, and AI predictions. (Refer to [Backend API Implementation Plan](/home/ubuntu/Backend_API_Implementation_Plan.md) for details).
*   **AI Model Integration:** Trained AI models are loaded and served via dedicated services within the Flask application, handling data preprocessing, inference, and postprocessing.

### 5.2 Frontend Web Application (React)

*   **Framework:** React 18 with Vite.
*   **Styling:** Tailwind CSS, shadcn/ui for components.
*   **RTL Support:** Native Arabic-first, RTL layout with appropriate CSS and component adjustments.
*   **Micro-Animations:** Framer Motion for smooth user interactions.
*   **Internationalization:** `react-i18next` for Arabic language support.
*   **Key Pages:** Onboarding, Dashboard, Meal Photo Analyzer, Insulin Calculator, History, Profile, Login/Signup. (Refer to [Frontend Web Application Development Plan](/home/ubuntu/Frontend_Web_Application_Development_Plan.md) for details).

## 6. Deployment and Monitoring

### 6.1 Deployment Strategy

*   **Platform:** Vercel for unified deployment of the Flask backend and React frontend.
*   **Dockerization:** Containerization ensures consistent environments.
*   **CI/CD:** GitHub Actions automates the build, test, and deployment pipeline.

### 6.2 Security

*   **HTTPS:** All communications are encrypted.
*   **JWT Authentication:** Secure user sessions.
*   **Input Validation & Rate Limiting:** Protect against common web vulnerabilities.
*   **Data Encryption:** At rest and in transit.

### 6.3 Monitoring and Logging

*   **Application Monitoring:** Sentry for error tracking and performance.
*   **User Session Monitoring:** LogRocket (optional) for detailed user interaction analysis.
*   **Database Monitoring:** Supabase built-in analytics.
*   **Logging:** Comprehensive logging in both backend and frontend for debugging and auditing.

(Refer to [System Deployment and Monitoring Setup Plan](/home/ubuntu/Deployment_and_Monitoring_Setup_Plan.md) for details).

## 7. Regulatory Compliance

The system is designed with adherence to SFDA regulations for AI/ML medical devices, emphasizing clinical validation, data privacy, and transparency in AI model operation.

## References

[1] Aldhirgham, T., et al. (2023). The Saudi branded food database: First-phase development (Branded Beverage Database). *Journal of Food Composition and Analysis*, *120*, 105299. https://doi.org/10.1016/j.jfca.2023.105299

[2] Bossard, L., Guillaumin, M., & Van Gool, L. (2014). Food-101 – Mining Discriminative Components with Random Forests. *ECCV*.

[3] Ege, T., et al. (2021). Nutrition5k: A Comprehensive Nutrition Dataset. *arXiv preprint arXiv:2103.04913*.

[4] IEEE Dataport. (n.d.). *Food Image Dataset*. Retrieved from https://ieee-dataport.org/keywords/food-image-dataset

[5] Al-Tawil, A. (n.d.). *Arabic Food 101*. Kaggle. Retrieved from https://www.kaggle.com/datasets/araraltawil/arabic-food-101

