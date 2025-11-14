# Student Performance Factors Analysis (Individual Formative Project)

### Project Overview and Objectives

This repository contains the deliverables for the Individual Formative Assignment: Python ETL and Data Visualisation. The project focuses on applying the data science pipeline to understand the relationship between key academic, social, and environmental factors in relation to student achievement.

The goal is to design a robust ETL pipeline, analyze the cleaned data, and present compelling, interactive visualizations to assist educators and researchers in strategic decision-making.

#### Key Objectives

- Design and Implement an ETL Pipeline: Clean, transform, and load dataset using Python/Pandas
- Visualise Data: Use Matplotlib, Seaborn, and Plotly to generate insights
- Process Documentation: Document all steps, challenges, and feature engineering rationale
- Key Finding: Determine the strongest positive and negative influences on Exam Score

## Technical Stack

- **Core Languages:** Python
- **Data Processing (ETL):** Pandas, NumPy, kagglehub
- **Visualisation:** Matplotlib (Basic Plotting), Seaborn (Statistical Visuals/Heatmaps), Plotly (Advanced Interactive Charts)
- **Version Control:** Git & GitHub

## ETL Pipeline and Transformation Summary

The data used is the [Student Performance Factors Dataset](https://www.kaggle.com/datasets/ayeshaseherr/student-performance/data), containing 6,607 records and 20 attributes, with the target variable being Exam_Score.

### 1. Data Cleaning and Imputation

The transformation phase focused on cleaning categorical values and handling missing data via Mode Imputation (the most appropriate statistical method for categorical columns). As the data is categorical, mode preserves the most statistically likely distribution without introducing distortion:

- **Teacher_Quality** (78 missing rows ≈ 1%): Imputed with the Mode: 'Medium'. 
- **Parental_Education_Level** (90 missing rows ≈ 1.3%): Imputed with the Mode: 'High School'.
- **Distance_from_Home** (67 missing rows ≈ 1%): Imputed with the Mode: 'Near'.

### 2. Encoding and Data Type Conversion

All ordinal categorical columns were converted to integers using a consistent ranking system to make them suitable for numerical correlation analysis.

| Column Name                 | Original Values                              | New Data Type | Encoding / New Value Representation                                               |
|-----------------------------|------------------------------------------------------------|---------------|------------------------------------------------------------------------------------|
| Parental_Involvement        | Low, Medium, High                           | Integer       | Ordinal Encoding: Low → 1, Medium → 2, High → 3                                   |
| Access_to_Resources         | Low, Medium, High                           | Integer       | Ordinal Encoding: Low → 1, Medium → 2, High → 3                                   |
| Motivation_Level            | Low, Medium, High                           | Integer       | Ordinal Encoding: Low → 1, Medium → 2, High → 3                                   |
| Family_Income               | Low, Medium, High                           | Integer       | Ordinal Encoding: Low → 1, Medium → 2, High → 3                                   |
| Teacher_Quality             | Low, Medium, High                           | Integer       | Ordinal Encoding: Low → 1, Medium → 2, High → 3                                   |
| Peer_Influence              | Negative, Neutral, Positive                 | Integer       | Ordinal Encoding: Negative → 1, Neutral → 2, Positive → 3                         |
| Parental_Education_Level    | High School, College, Postgraduate          | Integer       | Ordinal Encoding: High School → 1, College → 2, Postgraduate → 3                  |
| Distance_from_Home          | Near, Moderate, Far                         | Integer       | Ordinal Encoding: Near → 1, Moderate → 2, Far → 3                                 |
| Extracurricular_Activities  | Yes, No                                      | Boolean/Int   | Boolean Encoding: Yes → True, No → False                                          |
| Learning_Disabilities       | Yes, No                                      | Binary        | Binary Encoding: Yes → 1, No → 0                                                  |
| Internet_Access             | Yes, No                                      | Binary        | Binary Encoding: Yes → 1, No → 0                                                  |
| School_Type                 | Public, Private                              | Binary        | Binary Encoding: Public → 0, Private → 1                                          |
| Gender                      | Male, Female                                 | Binary        | Binary Encoding: Male → 0, Female → 1                                             |


### 3. Feature Engineering

Three features were created to synthesize complex relationships, driving the analysis and key findings:

| Engineered Feature | Purpose | Components | Rationale |
|---|---|---|---|
| Resource_Index | Summarises socioeconomic support | Family_Income, Internet_Access, Access_to_Resources | Used to test if resource access affects scores |
| Effort_Score | Captures total student effort | Hours_Studied, Attendance, Tutoring_Sessions | Expected strong positive correlation with score |
| Challenge_Index | Captures cumulative student disadvantage | Distance_from_Home, Learning_Disabilities, Inverted Parental_Involvement | Inversion Logic: Parental_Involvement was inverted (4 - value) so that a high score on the index correctly signifies high Challenge |

## Documented Challenges & Problem-Solving

Throughout the transformation phase, challenges were documented to demonstrate troubleshooting and resilience behaviours:

- **Encoding Halt/Troubleshooting:** An initial mapping issue caused the **Distance_from_Home** and **Access_to_Resources** columns to show all NaN values after encoding. The issue was resolved by restarting the Jupyter Kernel and re-running all cells, successfully clearing the memory state and allowing the ordinal mapping to complete.
- **Data Type Conversion:** The initial plan to use Boolean types for 'Yes/No' columns (like Learning_Disabilities), which I did, but later on I realised Binary (0/1) Integers would make the features more mathematically compatible for use in the Feature Engineering sums and the Correlation Matrix, so it was changed.

## Key Findings from Data Visualization

The visualization phase used three primary charts to answer the research questions, confirming the hypotheses driven by the engineered features.

### A. Correlation Matrix (Seaborn)

- **Strongest Positive Factor:** Effort_Score and Previous_Scores showed the highest positive correlation with Exam_Score.
- **Strongest Negative Factor:** Challenge_Index showed the strongest negative correlation (approx. -0.69) with Exam_Score.

### B. Categorical Impact (Box Plot)

**Insight:** This plot visually confirmed the negative correlation, showing that the Median Score generally decreases as the Challenge Group moves from 'Low Challenge' to 'High Challenge'.

### C. Advanced Interactive Scatter Plot (Plotly)

**Primary Story:** The plot demonstrated a clear division of students into two bands. While Effort is necessary for all, the top band (scores 80-100) is overwhelmingly dominated by students with a combination of high Previous Scores (large bubbles) and a Low Challenge Index (dark color).

**Conclusion:** Effort is necessary, but the highest achievement is easiest for those with high past performance and low challenge.

## How to Reproduce the Results

### Clone the Repository

```bash
git clone [https://github.com/Kanyin-Sola/IFP_Student_Performance_Analysis]
```

### Install Dependencies

Open your terminal or a Jupyter cell and run:

```bash
pip install pandas numpy seaborn matplotlib plotly kagglehub
```

### Run the Notebook

Open `spf_analysis.ipynb` and run all cells sequentially. The notebook will automatically download the data, run the ETL pipeline, and generate all visualizations.
