# AI Model Development and Training Plan

## Overview

This document details the development and training plan for the Artificial Intelligence (AI) models central to the AI-Powered Post-Meal Glucose Prediction System. The system will leverage a multi-model approach, combining computer vision for meal analysis with time-series prediction for glucose dynamics, and a fusion model for integrated predictions and insulin dose recommendations. The goal is to achieve 100% AI-based calculation accuracy for insulin doses, as emphasized in the project requirements.

## 1. Model Architectures

### 1.1 Meal Photo Analyzer: Food Recognition and Carbohydrate Estimation (CNN)

**Objective:** To accurately identify food items from meal photos and estimate their macronutrient content, particularly carbohydrates.

**Architecture:** Convolutional Neural Networks (CNNs) are well-suited for image recognition tasks. We will explore state-of-the-art architectures known for their performance and efficiency.

*   **ResNet (Residual Networks):** Known for solving the vanishing gradient problem in deep networks, allowing for very deep architectures. Variants like ResNet-50 or ResNet-101 can be used as a backbone for feature extraction.
*   **EfficientNet:** A family of models that uniformly scales all dimensions of depth, width, and resolution using a compound coefficient. EfficientNetB0-B7 offers a good balance between accuracy and computational cost.

**Input:** Meal photo (RGB image).

**Output:**
*   **Food Label:** Categorical classification of identified food items (e.g., "Kabsa", "Mandi", "Dates").
*   **Portion Size:** Regression output for estimated portion size (e.g., in grams or a categorical scale like small/medium/large).
*   **Macronutrient Estimation:** Regression outputs for estimated carbohydrates (grams), proteins (grams), and fats (grams).

**Loss Functions:**
*   **Cross-Entropy Loss:** For food item classification.
*   **Mean Squared Error (MSE) or Mean Absolute Error (MAE):** For portion size and macronutrient regression tasks.

**Equation Example (for Carbohydrate Estimation):**

$$ \text{Carbs} = f_{\text{CNN}}(\text{Image}) $$

Where $f_{\text{CNN}}$ represents the CNN model that processes the input image to output the estimated carbohydrate content.

### 1.2 Post-Meal Glucose Prediction (RNN/Transformer)

**Objective:** To predict the time-series of postprandial blood glucose levels based on historical glucose data, meal intake, and insulin doses.

**Architecture:** Recurrent Neural Networks (RNNs) and Transformer models are highly effective for sequential data processing.

*   **Long Short-Term Memory (LSTM) Networks:** A type of RNN capable of learning long-term dependencies, making them suitable for time-series prediction.
*   **Transformer Networks:** Originally designed for natural language processing, Transformers with their self-attention mechanisms have shown superior performance in various sequence-to-sequence tasks, including time-series forecasting. They can capture complex temporal relationships more effectively than traditional RNNs.

**Input:** A sequence of historical data points including:
*   Previous glucose readings (time-series).
*   Meal macronutrients (carbohydrates, proteins, fats) and timestamps.
*   Insulin doses (bolus, basal) and timestamps.
*   Activity levels, sleep, and stress (if available from the `activity_log` table).

**Output:** Predicted glucose values (mg/dL) at future time steps (e.g., every 15 minutes for the next 2-4 hours).

**Loss Function:** Mean Squared Error (MSE) or Mean Absolute Error (MAE) for regression.

**Equation Example (for Glucose Prediction):**

$$ \text{Glucose}_{t+1} = f_{\text{RNN/Transformer}}(\text{Glucose}_t, \text{Meal}_t, \text{Insulin}_t, \text{Activity}_t, \dots) $$

Where $f_{\text{RNN/Transformer}}$ represents the model predicting the next glucose value based on current and historical inputs.

### 1.3 Fusion Model: Integrated Prediction and Insulin Dose Calculation

**Objective:** To combine the outputs from the Meal Photo Analyzer and the Post-Meal Glucose Prediction model to provide a comprehensive postprandial glucose curve and calculate the recommended insulin dose.

**Architecture:** A neural network that takes the outputs of the CNN and RNN/Transformer models as its primary inputs. This could be a simple Multi-Layer Perceptron (MLP) or a more complex architecture depending on the interaction required.

**Input:**
*   Estimated carbohydrates from the Meal Photo Analyzer.
*   Predicted glucose curve from the RNN/Transformer model.
*   User-specific parameters: Carb Ratio (CR) and Correction Factor (CF).
*   Current glucose level.

**Output:**
*   **Final Predicted Postprandial Glucose Curve:** A refined time-series prediction.
*   **Recommended Insulin Dose:** A single numerical value for the bolus insulin dose.

**Insulin Dose Calculation (100% AI-driven and medically aligned):**

The insulin dose will be calculated using the standard formula, but with all parameters derived or refined by AI models.

$$ \text{Insulin Dose} = \left( \frac{\text{Estimated Carbs}}{\text{Carb Ratio}} \right) + \left( \frac{\text{Current Glucose} - \text{Target Glucose}}{\text{Correction Factor}} \right) $$

Where:
*   `Estimated Carbs` is derived from the CNN model.
*   `Current Glucose` is the latest reading, potentially refined by the RNN/Transformer.
*   `Carb Ratio` and `Correction Factor` are user-specific, but their optimal values could be dynamically adjusted or validated by the AI based on historical data and glucose responses.

**Loss Function:** A multi-task loss function combining MSE/MAE for glucose prediction and potentially a custom loss for insulin dose accuracy, possibly incorporating medical constraints or penalties for over/under-dosing.

## 2. Training Methodology

### 2.1 Data Splitting

The collected and simulated datasets will be split into training, validation, and test sets (e.g., 70% training, 15% validation, 15% test) to ensure robust model evaluation and prevent overfitting.

### 2.2 Hyperparameter Tuning

Techniques like grid search, random search, or Bayesian optimization will be used to find optimal hyperparameters for each model (e.g., learning rate, batch size, number of layers, hidden units).

### 2.3 Optimization

Adam optimizer with a dynamic learning rate schedule (e.g., learning rate decay, ReduceLROnPlateau) will be employed.

### 2.4 Early Stopping

Training will incorporate early stopping based on validation loss to prevent overfitting and optimize training time.

## 3. Evaluation Metrics

Rigorous evaluation is crucial, especially for a medical device. The following metrics will be used:

### 3.1 For Glucose Prediction (Regression Models)

*   **Mean Absolute Error (MAE):** Measures the average magnitude of the errors in a set of predictions, without considering their direction. It is the average over the test sample of the absolute differences between prediction and actual observation where all individual differences have equal weight.
    $$ \text{MAE} = \frac{1}{N} \sum_{i=1}^{N} |y_i - \hat{y}_i| $$
*   **Root Mean Squared Error (RMSE):** Measures the average magnitude of the errors. It is the square root of the average of the squared differences between prediction and actual observation. RMSE gives a relatively high weight to large errors.
    $$ \text{RMSE} = \sqrt{\frac{1}{N} \sum_{i=1}^{N} (y_i - \hat{y}_i)^2 } $$
*   **Mean Absolute Relative Difference (MARD):** A key metric for Continuous Glucose Monitoring (CGM) accuracy, comparing predicted glucose values to reference values. Lower MARD indicates higher accuracy. This will be used to compare against simulated clinical CGM accuracy.
    $$ \text{MARD} = \frac{1}{N} \sum_{i=1}^{N} \frac{|y_i - \hat{y}_i|}{y_i} \times 100\% $$

### 3.2 For Hypo/Hyperglycemia Prediction (Classification Component of Early Warning Engine)

*   **Sensitivity (Recall):** The proportion of actual positive cases that are correctly identified as positive (e.g., correctly predicting hypoglycemia when it occurs).
    $$ \text{Sensitivity} = \frac{\text{True Positives}}{\text{True Positives} + \text{False Negatives}} $$
*   **Specificity:** The proportion of actual negative cases that are correctly identified as negative (e.g., correctly predicting no hypoglycemia when it doesn't occur).
    $$ \text{Specificity} = \frac{\text{True Negatives}}{\text{True Negatives} + \text{False Positives}} $$
*   **Precision:** The proportion of positive identifications that were actually correct.
    $$ \text{Precision} = \frac{\text{True Positives}}{\text{True Positives} + \text{False Positives}} $$
*   **F1-Score:** The harmonic mean of precision and recall, providing a balance between the two.
    $$ \text{F1-Score} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}} $$

### 3.3 Cross-Validation and Baseline Comparison

*   **K-Fold Cross-Validation:** To ensure model generalization and robustness, especially with limited real-world data.
*   **Baseline Models:** Performance will be compared against simpler baseline models (e.g., linear regression, ARIMA for time-series) to demonstrate the value of the AI approach.

### 3.4 Confidence Intervals and Uncertainty Estimates

For all predictions, especially glucose levels and insulin doses, confidence intervals and uncertainty estimates will be provided. This is critical for clinical decision-making and aligns with regulatory requirements for AI/ML medical devices, allowing users and clinicians to understand the reliability of the predictions.

## 4. Verification and Validation

Model verification and validation will be an ongoing process, including:

*   **Test Dataset Validation:** Models will be rigorously tested on an independent test dataset that was not used during training or validation.
*   **Simulated Clinical Accuracy:** The system's performance will be compared against simulated clinical CGM accuracy standards (e.g., MARD values typically accepted for commercial CGMs).
*   **100% AI-based Computation of Insulin Dose:** Special emphasis will be placed on ensuring that the insulin dose calculation is entirely driven by AI models and medically aligned, as per the project's core requirement.

## 5. Next Steps

Upon completion of this phase, the trained model weights and evaluation reports will be prepared for integration into the backend API. The next phase will focus on **Backend API Development and Integration**.
