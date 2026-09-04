# Outlier Detection Lab

## 📌 Overview

This project demonstrates how to **detect, analyze, and treat outliers** in an e-commerce dataset using Python.

The lab focuses on understanding when an unusual value should be removed, capped, or preserved as useful information. The analysis is performed using **Pandas, NumPy, and SciPy**.

## 🎯 Objectives

* Profile the dataset using statistical summaries.
* Compare mean and median to understand data distributions.
* Detect outliers using **Z-score** and **IQR** methods.
* Apply three different outlier treatment strategies:

  * Remove
  * Cap/Floor (Winsorization)
  * Flag as a feature
* Compare the data before and after treatment.
* Understand how business context affects the choice of outlier treatment.

## 🛠️ Technologies Used

* Python
* Pandas
* NumPy
* SciPy
* Google Colab
* Jupyter Notebook

## 📊 Dataset

The dataset contains 800 synthetic e-commerce orders with three main columns:

| Column           | Description                       |
| ---------------- | --------------------------------- |
| `purchase_value` | Value of the order                |
| `delivery_days`  | Number of days taken for delivery |
| `review_score`   | Customer review score from 1–5    |

The dataset intentionally contains extreme values so that different outlier treatment strategies can be demonstrated.

## 🔍 Outlier Detection Methods

### 1. Z-score

Z-score measures how many standard deviations a value is away from the mean.

A value is flagged as an outlier when:

```text
|Z| > 3
```

This method was applied to `delivery_days`, which is approximately symmetric.

### 2. IQR

The Interquartile Range is calculated as:

```text
IQR = Q3 - Q1
```

The outlier boundaries are:

```text
Lower Fence = Q1 - 1.5 × IQR
Upper Fence = Q3 + 1.5 × IQR
```

Values outside these boundaries are flagged as outliers.

IQR was applied to `purchase_value` because the column is right-skewed.

##  Outlier Treatment Strategies

### Remove

Used for `delivery_days` after the extreme values were confirmed to be system errors.

The affected rows were removed from the cleaned dataset.

### Cap/Floor

Used for `purchase_value`.

The high-value orders were confirmed to be genuine bulk transactions, so they were not deleted. Instead, extreme values were capped at the IQR upper fence using Pandas `clip()`.

### Flag as a Feature

A new Boolean feature was created:

```python
is_high_value_order
```

It identifies whether an order's purchase value is above the IQR upper fence.

This keeps the original purchase value unchanged while providing additional information that a machine-learning model could use.

##  Key Learning

An outlier is not automatically an error.

The correct treatment depends on what the value represents:

```text
Incorrect / noise
        ↓
     Remove

Real but disruptive
        ↓
       Cap

Real and informative
        ↓
       Flag
```

The project demonstrates that **business context is essential when deciding how to treat outliers**.

##  Project Contents

```text
Outlier-Detection-Lab/
│
├── Outlier_Detection_Lab.ipynb
└── README.md
```

##  Conclusion

This lab demonstrates the complete process of working with outliers:

**Profile → Detect → Diagnose → Treat → Verify**

The analysis shows how different treatment strategies affect the dataset and why the choice of treatment should be based on both statistical evidence and the meaning of the data.
