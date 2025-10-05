# Data Collection and Preparation Plan for AI-Powered Post-Meal Glucose Prediction System

## Overview

This document outlines the strategy for data collection and preparation, which is a critical phase for developing the AI-Powered Post-Meal Glucose Prediction System. The success of the Meal Photo Analyzer and Post-Meal Insulin Calculator heavily relies on the quality, relevance, and comprehensiveness of the datasets. Special attention will be given to Saudi Arabian cuisine and dietary patterns.

## 1. Existing Data Sources and Their Limitations

### 1.1 Saudi Branded Food Database (SBFD)

Research into existing data sources identified the **Saudi Branded Food Database (SBFD)** as a significant initiative. The first phase of its development, the Branded Beverage Database (BBD), collected data on 1748 beverages available in Saudi Arabian markets between June and October 2021 [1]. The SBFD aims to provide a comprehensive reference for the nutrient content of pre-packed foods and beverages, using food label information and manufacturer-provided data. Its purpose is to support nutrition epidemiological research, national dietary intake assessment, and monitoring of the food and drink marketplace.

**Limitations for this project:**

*   **Focus on Branded Products:** The SBFD primarily focuses on pre-packed, branded food and beverage products, which may not adequately cover traditional Saudi home-cooked meals or restaurant dishes like Kabsa, Mandi, and other local specialties.
*   **Beverage-centric First Phase:** The initial phase (BBD) is limited to beverages, meaning comprehensive food item data is still under development.
*   **Accessibility:** While the SBFD aims to be available to regulators, researchers, and the general community, direct programmatic access or a readily available dataset for AI model training was not immediately apparent from the research.

### 1.2 General Food Image and Nutrition Datasets

Several public datasets like Food-101 [2], Nutrition5k [3], and various IEEE Dataport food image datasets [4] exist for food recognition and nutritional estimation. Some datasets specifically focus on Middle Eastern cuisine, such as 

the Arabic Food 101 dataset [5] and the Mediterranean Food Image Recognition dataset [6].

**Limitations for this project:**

*   **Lack of Saudi-Specific Data:** These datasets may not have sufficient representation of Saudi-specific dishes, which is a key requirement for the project.
*   **Incomplete Nutritional Information:** Many food image datasets lack detailed and verified nutritional information, especially for portion sizes and macronutrient breakdowns.
*   **No Postprandial Glucose Data:** None of the publicly available datasets include simulated or actual postprandial glucose data, which is essential for training the glucose prediction model.

## 2. Data Collection and Generation Strategy

Given the limitations of existing datasets, a multi-pronged approach will be adopted to create a comprehensive and relevant dataset for the project.

### 2.1 Protocol for Meal Photo Collection

A protocol will be established for collecting high-quality meal photos. This will involve:

*   **Standardized Photo Capture:** Guidelines for lighting, camera angle (top-down and 45-degree views), and background to ensure consistency.
*   **Multiple Portion Sizes:** Capturing images of the same dish with different portion sizes (e.g., small, medium, large) to train the portion estimation model.
*   **Data Annotation:** Each image will be annotated with the food label, portion size, and a detailed list of ingredients.

### 2.2 Simulated Postprandial Glucose Data

Since collecting real postprandial glucose data from human subjects is outside the scope of this project, a simulation-based approach will be used. This will involve:

*   **Nutritional Analysis:** For each meal photo, the macronutrient content (carbohydrates, proteins, fats) will be estimated based on the ingredients and portion size. This can be done using nutritional databases or by consulting with a nutritionist.
*   **Glucose Simulation Model:** A physiological model will be developed to simulate the postprandial glucose response based on the macronutrient content, user's physiological parameters (e.g., insulin sensitivity), and other factors. This model will be based on established physiological principles and validated against existing literature.
*   **Data Generation:** The simulation model will be used to generate a time-series of simulated postprandial glucose readings for each meal photo, creating a paired dataset of (image, nutritional info, glucose curve).

### 2.3 Synthetic Data Augmentation

To increase the size and diversity of the training dataset, synthetic data augmentation techniques will be employed:

*   **Image Augmentation:** Applying transformations like rotation, scaling, cropping, and color jittering to the collected meal photos to create variations.
*   **Generative Adversarial Networks (GANs):** Potentially using GANs to generate new, realistic images of Saudi dishes to further expand the dataset.

## 3. Database Schema

A PostgreSQL database will be used to store the collected and generated data. The following database schema is proposed:

**`users` table:**
*   `id` (UUID, primary key)
*   `email` (VARCHAR, unique)
*   `password_hash` (VARCHAR)
*   `age` (INTEGER)
*   `weight_kg` (FLOAT)
*   `height_cm` (FLOAT)
*   `diabetes_type` (VARCHAR)
*   `carb_ratio` (FLOAT)
*   `correction_factor` (FLOAT)
*   `target_glucose_mg_dl` (INTEGER)
*   `created_at` (TIMESTAMP)

**`meals` table:**
*   `id` (UUID, primary key)
*   `user_id` (UUID, foreign key to `users.id`)
*   `image_path` (VARCHAR)
*   `food_label` (VARCHAR)
*   `portion_size` (VARCHAR)
*   `estimated_carbs_g` (FLOAT)
*   `estimated_protein_g` (FLOAT)
*   `estimated_fat_g` (FLOAT)
*   `estimated_calories` (FLOAT)
*   `timestamp` (TIMESTAMP)

**`simulated_cgm` table:**
*   `id` (UUID, primary key)
*   `meal_id` (UUID, foreign key to `meals.id`)
*   `timestamp` (TIMESTAMP)
*   `glucose_value_mg_dl` (FLOAT)

**`insulin_log` table:**
*   `id` (UUID, primary key)
*   `user_id` (UUID, foreign key to `users.id`)
*   `meal_id` (UUID, foreign key to `meals.id`, nullable)
*   `bolus_dose` (FLOAT)
*   `basal_dose` (FLOAT, nullable)
*   `timestamp` (TIMESTAMP)

**`activity_log` table:**
*   `id` (UUID, primary key)
*   `user_id` (UUID, foreign key to `users.id`)
*   `activity_type` (VARCHAR)
*   `duration_minutes` (INTEGER)
*   `intensity` (VARCHAR)
*   `timestamp` (TIMESTAMP)

## 4. Data Labeling Instructions

To ensure high-quality data for model training, the following labeling instructions will be followed:

*   **Food Labeling:** Each meal image will be labeled with the name of the dish in both Arabic and English (e.g., "Kabsa - كبسة").
*   **Ingredient Tagging:** A list of all visible ingredients will be tagged for each meal.
*   **Portion Size Annotation:** Portion sizes will be categorized as small, medium, or large based on a visual reference guide.
*   **Data Verification:** A two-step verification process will be implemented, where a second annotator reviews and confirms the labels assigned by the first annotator.

## 5. Next Steps

With the data collection and preparation plan established, the next steps will involve:

1.  **Developing the data collection tools and infrastructure.**
2.  **Initiating the meal photo collection process.**
3.  **Building and validating the glucose simulation model.**
4.  **Setting up the data annotation pipeline.**
5.  **Populating the database with the collected and generated data.**

This comprehensive data collection and preparation plan will provide a solid foundation for training the AI models in the subsequent phases of the project.

## References

[1] Aldhirgham, T., et al. (2023). The Saudi branded food database: First-phase development (Branded Beverage Database). *Journal of Food Composition and Analysis*, *120*, 105299. https://doi.org/10.1016/j.jfca.2023.105299

[2] Bossard, L., Guillaumin, M., & Van Gool, L. (2014). Food-101 – Mining Discriminative Components with Random Forests. *ECCV*.

[3] Ege, T., et al. (2021). Nutrition5k: A Comprehensive Nutrition Dataset. *arXiv preprint arXiv:2103.04913*.

[4] IEEE Dataport. (n.d.). *Food Image Dataset*. Retrieved from https://ieee-dataport.org/keywords/food-image-dataset

[5] Al-Tawil, A. (n.d.). *Arabic Food 101*. Kaggle. Retrieved from https://www.kaggle.com/datasets/araraltawil/arabic-food-101

[6] Konstantakopoulos, F. S., et al. (2021). Mediterranean Food Image Recognition Using Deep Learning. *2021 IEEE 22nd International Workshop on Multimedia Signal Processing (MMSP)*, 1-6. https://doi.org/10.1109/MMSP53017.2021.9630481

