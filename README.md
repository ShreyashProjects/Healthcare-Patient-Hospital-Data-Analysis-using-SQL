# Healthcare-Patient-Hospital-Data-Analysis-using-SQL

## Project Overview

This project focuses on analyzing healthcare patient and hospital data using SQL to extract meaningful insights. The dataset contains patient demographics, hospital admission details, medical conditions, billing information, and treatment data. The objective of this project is to perform data cleaning, exploratory analysis, and business-focused analysis to understand healthcare trends and hospital performance.

## Objectives

* Clean and prepare healthcare data for analysis.
* Perform patient demographic analysis.
* Analyze hospital admissions and patient distribution.
* Identify the most common medical conditions and disease patterns.
* Evaluate hospital billing and treatment costs.
* Generate business insights that could help hospitals improve resource planning and operational efficiency.

## Dataset Description

The dataset includes the following attributes:

* Name
* Age
* Gender
* Blood Type
* Medical Condition
* Date of Admission
* Doctor
* Hospital
* Insurance Provider
* Billing Amount
* Room Number
* Admission Type
* Discharge Date
* Medication
* Test Results

These attributes allow analysis across multiple dimensions such as patient demographics, disease patterns, hospital workload, and financial performance.

## Key Analysis Performed

### Data Cleaning

* Identified and removed duplicate records.
* Detected and handled missing values in important fields.
* Standardized inconsistent categorical values such as gender.
* Validated patient ages and removed unrealistic values.

### Patient Demographic Analysis

* Distribution of patients by age and gender.
* Blood group distribution among patients.
* Identification of dominant patient age groups.
* Analysis of elderly patient percentage.

### Hospital Admission Analysis

* Yearly and monthly patient admission trends.
* Hospitals with the highest patient inflow.
* Most common admission types and reasons.
* Average hospital stay duration.

### Disease Analysis

* Most common medical conditions affecting patients.
* Disease distribution across different age groups.
* Gender-based disease patterns.
* Diseases most common among elderly patients.

### Financial Analysis

* Total hospital revenue generated.
* Average billing amount per patient.
* Medical conditions generating the highest revenue.
* Hospitals contributing the most to total billing.

### Advanced SQL Analysis

* Ranking hospitals based on revenue generation.
* Identifying the most expensive treatments.
* Running revenue totals over time.
* Disease ranking within different age groups.

## Business Insights

The analysis provides insights that can help healthcare organizations:

* Understand patient demographics and disease distribution.
* Identify peak admission periods and hospital workload.
* Optimize hospital resource allocation.
* Detect high-cost treatments and revenue-generating conditions.
* Improve healthcare planning and operational decision-making.

## Tools Used

* MySQL
* SQL (Joins, Aggregations, Window Functions, CTEs, Data Cleaning Queries)

Q1. How many total patient records are present?

Answer:
SELECT COUNT(*) AS total_patients
FROM healthcare;

Q2. What are the different genders available in the dataset?

Answer:
SELECT DISTINCT gender
FROM healthcare;

Q3. What is the average age of patients?

Answer:
SELECT AVG(age) AS average_age
FROM healthcare;

Q4. What is the minimum and maximum age of patients?

Answer:
SELECT MIN(age) AS minimum_age,
MAX(age) AS maximum_age
FROM healthcare;

Q5. How many patients belong to each blood group?

Answer:
SELECT blood_type,
COUNT(*) AS patient_count
FROM healthcare
GROUP BY blood_type
ORDER BY patient_count DESC;

Data Cleaning
Q6. Check if duplicate patient records exist.

Answer:
SELECT name, age, gender, blood_type, medical_condition,
COUNT(*) AS duplicate_count
FROM healthcare
GROUP BY name, age, gender, blood_type, medical_condition
HAVING COUNT(*) > 1;

Q7. Identify rows where important fields are NULL.

Answer:
SELECT *
FROM healthcare
WHERE age IS NULL
OR gender IS NULL
OR medical_condition IS NULL
OR hospital IS NULL;

Q8. Find inconsistent gender values.

Answer:
SELECT DISTINCT gender
FROM healthcare;

Q9. Identify invalid ages.

Answer:
SELECT *
FROM healthcare
WHERE age < 0 OR age > 120;

Q10. Standardize gender values.

Answer:
UPDATE healthcare
SET gender =
CASE
WHEN gender IN ('M','Male') THEN 'Male'
WHEN gender IN ('F','Female') THEN 'Female'
ELSE gender
END;

Patient Analysis
Q11. Count patients by gender.

Answer:
SELECT gender,
COUNT(*) AS total_patients
FROM healthcare
GROUP BY gender;

Q12. Count patients by blood group.

Answer:
SELECT blood_type,
COUNT(*) AS total_patients
FROM healthcare
GROUP BY blood_type;

Q13. Which age group has the most patients?

Answer:
SELECT
CASE
WHEN age BETWEEN 0 AND 18 THEN '0-18'
WHEN age BETWEEN 19 AND 35 THEN '19-35'
WHEN age BETWEEN 36 AND 60 THEN '36-60'
ELSE '60+'
END AS age_group,
COUNT(*) AS patient_count
FROM healthcare
GROUP BY age_group
ORDER BY patient_count DESC;

Q14. What percentage of patients are above age 60?

Answer:
SELECT
(COUNT(CASE WHEN age > 60 THEN 1 END) * 100.0 / COUNT(*))
AS percent_above_60
FROM healthcare;

Hospital / Admission Analysis
Q15. How many patients were admitted each year?

Answer:
SELECT YEAR(date_of_admission) AS admission_year,
COUNT(*) AS total_patients
FROM healthcare
GROUP BY admission_year
ORDER BY admission_year;

Q16. Which hospital has the highest number of patients?

Answer:
SELECT hospital,
COUNT(*) AS patient_count
FROM healthcare
GROUP BY hospital
ORDER BY patient_count DESC
LIMIT 1;

Q17. What are the top 5 most common admission types?

Answer:
SELECT admission_type,
COUNT(*) AS total
FROM healthcare
GROUP BY admission_type
ORDER BY total DESC
LIMIT 5;

Q18. Which month has the highest number of admissions?

Answer:
SELECT MONTH(date_of_admission) AS month,
COUNT(*) AS total_admissions
FROM healthcare
GROUP BY month
ORDER BY total_admissions DESC;

Q19. What is the average hospital stay duration?

Answer:
SELECT AVG(DATEDIFF(discharge_date, date_of_admission))
AS avg_stay_days
FROM healthcare;

Disease / Diagnosis Analysis
Q20. What are the top 10 most common medical conditions?

Answer:
SELECT medical_condition,
COUNT(*) AS total_cases
FROM healthcare
GROUP BY medical_condition
ORDER BY total_cases DESC
LIMIT 10;

Q21. Which disease affects the highest number of patients?

Answer:
SELECT medical_condition,
COUNT(*) AS patient_count
FROM healthcare
GROUP BY medical_condition
ORDER BY patient_count DESC
LIMIT 1;

Q22. Which gender is more affected by specific diseases?

Answer:
SELECT medical_condition,
gender,
COUNT(*) AS total_cases
FROM healthcare
GROUP BY medical_condition, gender
ORDER BY total_cases DESC;

Billing & Cost Analysis
Q23. What is the total hospital billing amount?

Answer:
SELECT SUM(billing_amount) AS total_revenue
FROM healthcare;

Q24. What is the average billing amount per patient?

Answer:
SELECT AVG(billing_amount) AS avg_billing
FROM healthcare;

Q25. Which medical condition generates the highest revenue?

Answer:
SELECT medical_condition,
SUM(billing_amount) AS total_revenue
FROM healthcare
GROUP BY medical_condition
ORDER BY total_revenue DESC
LIMIT 1;

Q26. Which hospital generates the most revenue?

Answer:
SELECT hospital,
SUM(billing_amount) AS revenue
FROM healthcare
GROUP BY hospital
ORDER BY revenue DESC
LIMIT 1;

Advanced SQL
Q27. Rank hospitals based on total revenue generated.

Answer:
SELECT hospital,
SUM(billing_amount) AS total_revenue,
RANK() OVER (ORDER BY SUM(billing_amount) DESC) AS hospital_rank
FROM healthcare
GROUP BY hospital;

Q28. Find the top 5 most expensive patient treatments.

Answer:
SELECT name,
medical_condition,
billing_amount
FROM healthcare
ORDER BY billing_amount DESC
LIMIT 5;

Q29. Calculate running total of revenue over time.

Answer:
SELECT date_of_admission,
SUM(billing_amount) OVER (ORDER BY date_of_admission) AS running_revenue
FROM healthcare;

Q30. Identify patients whose treatment cost is above average.

Answer:
SELECT *
FROM healthcare
WHERE billing_amount >
(SELECT AVG(billing_amount) FROM healthcare);

## Project Outcome

This project demonstrates how SQL can be used to clean, analyze, and derive business insights from healthcare data. It highlights practical data analysis skills such as data cleaning, aggregation, ranking, and trend analysis, which are essential for entry-level data analyst roles.
