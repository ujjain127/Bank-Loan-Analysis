# 💼 Bank Loan Applications Dashboard (Power BI Project)

This project provides an end-to-end Power BI solution for analyzing and visualizing bank loan applications. It includes data cleaning, transformation, DAX measures, and an interactive dashboard to explore loan approvals, credit scores, employment trends, and repayment history.

## 📂 Dataset Files

- `Applicants.csv`  
  Fields: `ApplicantID`, `Name`, `Age`, `EmploymentStatus`, `CreditScore`

- `Applications.csv`  
  Fields: `ApplicationID`, `ApplicantID`, `LoanType`, `Amount`, `Status`, `ApplicationDate`

- `LoanHistory.csv`  
  Fields: `ApplicantID`, `LoanID`, `PastLoanAmount`, `PaidOnTime`

> Each file contains **100+ records** for analysis and modeling.

---

## 🧼 Data Cleaning & Transformation (Power Query)

- Removed duplicates
- Fixed data types (e.g., `CreditScore` as Whole Number, `Amount` as Currency)
- Handled missing values:
  - Null `EmploymentStatus` → `"Unknown"`
  - Null `CreditScore` → Replaced with average
- Text cleanup:
  - Used `Text.Proper()` for names
  - Trimmed spaces
  - Replaced `"Self Employed"` → `"Self-Employed"`

---

## 🔗 Data Modeling

- Relationships:
  - `Applicants[ApplicantID]` → `Applications[ApplicantID]`
  - `Applicants[ApplicantID]` → `LoanHistory[ApplicantID]`
- Cardinality: One-to-Many (1:*)

---

## 📊 DAX Measures

```DAX
LoanApprovalRate = 
DIVIDE(
    CALCULATE(COUNTROWS(Applications), Applications[Status] = "Approved"),
    COUNTROWS(Applications)
)

AvgLoanAmount = AVERAGE(Applications[Amount])

OnTimeRate = 
DIVIDE(
    CALCULATE(COUNTROWS(LoanHistory), LoanHistory[PaidOnTime] = "Yes"),
    COUNTROWS(LoanHistory)
)

CreditScoreCategory = 
SWITCH(TRUE(),
    Applicants[CreditScore] >= 750, "Excellent",
    Applicants[CreditScore] >= 650, "Good",
    Applicants[CreditScore] >= 550, "Fair",
    "Poor"
)
