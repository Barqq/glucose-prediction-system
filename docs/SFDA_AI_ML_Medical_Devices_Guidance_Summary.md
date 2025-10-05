# Summary of SFDA Guidance on AI and Machine Learning based Medical Devices (MDS-G010)

This document summarizes the key aspects of the Saudi Food and Drug Authority (SFDA) Guidance on Artificial Intelligence (AI) and Machine Learning (ML) technologies based Medical Devices (MDS-G010), Version 1.0, dated 29/11/2022. This guidance is crucial for understanding the regulatory landscape for the development and deployment of the AI-Powered Post-Meal Glucose Prediction System in Saudi Arabia.

## 1. Purpose and Scope

**Purpose:** The guidance clarifies the requirements for obtaining Medical Devices Marketing Authorization (MDMA) for AI and ML-based medical devices to be placed on the market within the Kingdom of Saudi Arabia (KSA).

**Scope:** It applies to AI and ML technologies that **diagnose, manage, or predict diseases by analyzing medical data**.

## 2. Medical Device Classification and Criteria

AI/ML technologies are classified as medical devices if their **intended use** is for investigation, detection, diagnosis, monitoring, treatment, or management of any medical condition, disease, anatomy, or physiological process. Examples include in-vitro diagnostic tools and AI-based biosensors that predict disease tendencies.

## 3. Premarket Review Considerations

Manufacturers of AI/ML-based medical devices are expected to meet technical documentation requirements for MDMA, as specified in MDS-REQ 1, Annex (3) Medical Device Technical Documentation. Key documentation includes:

*   Device Description and Specification
*   Information to be provided by the Manufacturer
*   Design and Manufacturing Information
*   Essential Principles of Safety and Performance
*   Benefit-Risk Analysis and Risk Management
*   Product Verification and Validation
*   Post Market Surveillance Plan
*   Periodic Safety Update Report and Post Market Surveillance Report

Special consideration is given to **verification and validation testing**, which must demonstrate that the device meets design specifications, performs as intended, includes usability studies, and performs safely under normal and abnormal conditions.

## 4. Clinical Evaluation

The SFDA emphasizes that there is no internationally aligned framework for clinical evaluation of AI/ML-based medical devices. Manufacturers must provide **clinical evidence of safety, effectiveness, and performance** before market placement. This involves:

*   **Valid Clinical Association:** Evidence that the device output is clinically accepted based on scientific literature, original clinical research, and/or clinical guidelines. New evidence may need to be generated if existing evidence is insufficient.
*   **Analytical/Technical Validation:** Evaluation of the correctness of input data processing to create reliable output data, demonstrating that the device meets its specifications. This typically involves using labeled reference datasets.
*   **Clinical Validation:** Measures the ability of the device to yield a clinically meaningful outcome in the target population. This is evaluated both pre-market and post-market. Metrics may include specificity, sensitivity, positive predictive value (PPV), negative predictive value (NPV), and clinical usability. Independent review of clinical evaluation results may be required.

## 5. Risk Management

Risk management is a critical component, requiring manufacturers to:

*   Identify, analyze, evaluate, control, and monitor risks associated with the device throughout its lifecycle.
*   Address risks related to data quality, operational controls, human-user interface design (to avoid bias), and hand-off strategies for autonomous systems.
*   Perform risk management review according to Clause 9 of ISO 14971:2019.

## 6. Quality Management Systems (QMS)

AI/ML devices must be designed, manufactured, and monitored in accordance with **Medical Devices Quality Management System (ISO 13485)**. The QMS must ensure:

*   Compliance with regulations.
*   Competent human resources with technology, software engineering, and clinical aspects knowledge.
*   Available infrastructure (equipment, networks, tools).
*   Traceability of the AI/ML system and its development, including configuration and change management.
*   Measurement and Monitoring: Post-market surveillance, logging complaints, addressing technical issues, and reporting adverse events to the SFDA.

## 7. Change Notification

Manufacturers must inform the SFDA of any **significant or non-significant changes** to the device within specified timeframes (10 days for significant, 30 days for non-significant). All changes must be evaluated, verified, and validated according to the manufacturer's QMS.

## Implications for the AI-Powered Post-Meal Glucose Prediction System

Given that the proposed system aims to **predict postprandial blood glucose and insulin dose** and involves **100% AI-based calculation accuracy**, it will undoubtedly be classified as an AI/ML-based medical device by the SFDA. This implies several critical considerations for the project:

*   **Regulatory Compliance:** Strict adherence to MDS-G010 and related SFDA regulations (MDS-REQ 1, cybersecurity guidances) will be paramount for market authorization.
*   **Clinical Validation:** The 

system will require rigorous clinical validation, including demonstrating valid clinical association, analytical/technical validation, and clinical validation in the target population. This will likely involve generating new clinical data and comparing performance against established metrics.
*   **Data Requirements:** The guidance emphasizes the need for large, independent reference datasets that reflect the intended purpose and diversity of the target population, sourced from multiple medical centers, and with demographic/socio-economic characteristics corresponding to the target region (Saudi Arabia).
*   **Risk Management & QMS:** A robust Quality Management System (ISO 13485 compliant) and comprehensive risk management plan will be essential throughout the development lifecycle, covering data integrity, human-computer interaction, and post-market surveillance.
*   **Transparency and Documentation:** Detailed documentation of model architecture, training, evaluation, and changes will be necessary for regulatory submission.

This initial research confirms the significant regulatory considerations for the project, particularly regarding clinical validation and data requirements. These aspects must be integrated into the project plan from the outset to ensure successful market entry in Saudi Arabia.
