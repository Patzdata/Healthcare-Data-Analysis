# Healthcare-Data-Analysis

## Project Overview
The financial planning department at a Rwandan clinic needs to forecast their budget. Since flu season significantly impacts patient visits and treatment costs, this project focuses on:

- Cleaning and standardizing messy health data (dates, costs, age, gender, names).

- Extracting trends in flu cases and costs across months.

- Identifying the peak flu season to support budget planning.

This is my step-by-step thought log of cleaning and analyzing the patient records dataset.  
I kept it like a personal diary so others can follow my journey mistakes, fixes, and all.  

### Step 1: Creating and Importing Data
- Created a database called `Health_care.dbo` in SQL Server.
- Loaded `patient_record.csv` into SQL Server as `patient_record`.
- Columns included: `patient_id, age, gender, cost, visit_date, diagnosis`, 'treatment_type', 'doctor_id'.
- Also import 'patient_mapping' where i was able to clean the 'patient_name'.

---

### Step 2: Cleaning Age
- Issues found:
  - Negative values (e.g., `-5`).
  - Words instead of numbers (e.g., `thirty-five`).
  - Unrealistic ages (e.g., `150`).
- Solution:
  - Created new column `AgeClean`.
  - Used `CASE`, `TRY_CAST`, and a helper function for words → numbers.
  - Replaced invalid ages with `NULL`.
- Lesson: always keep original column untouched for reference.

---

### Step 3: Cleaning Gender
- Issues found:
  - Mixed values like `Male, M, Female, F, 123, ?`.
- Solution:
  - Created `GenderClean` column.
  - Standardized everything to only **Male** or **Female**.
  - Non-valid entries (e.g., `123`, `?`) set to `NULL`.
- Mistake: I once overwrote the original column . Learned to always create a new one.

---

### Step 4: Cleaning Cost
- Issues found:
  - Non-numeric entries like `FREE, ERROR, MISSING, TWO HUNDRED`.
  - Commas inside numbers (e.g., `1,250.00`).
- Solution:
  - Created `Cost_Clean` column.
  - Used `TRY_CAST` for numeric values and removed commas.
  - Used `fn_WordsToNumber_Cost` for text numbers (e.g., `two hundred → 200`).
  - Set invalid values (`FREE`, `ERROR`, `MISSING`) to `NULL`.

---

### Step 5: Cleaning Visit Date
- Issues found:
  - Datetime format: `1960-06-16 00:00:00.0000000`.
- Solution:
  - Extracted just the date with `TRY_CONVERT(date, visit_date, 120)`.
  - Stored in new column `VisitDate_Clean`.

---

### Step 6: Cleaning Patient Names
- Issues found:
  - Some names were all uppercase, some all lowercase.
- Solution:
  - Converted names to **Proper Case** (first letter capitalized).
  - Stored in `PatientName_Clean`.

---

### Step 7: Analysis
- After cleaning, ran analysis:
  - Counted flu cases per year and month.
  - Summed treatment costs.
  - Identified peak flu month and average costs.
 
<img width="1002" height="549" alt="image" src="https://github.com/user-attachments/assets/28acffcd-ab96-4397-a0f9-246da24aa417" />
---
<img width="991" height="543" alt="image" src="https://github.com/user-attachments/assets/83a2d51e-8247-4bbe-9f69-b80858fc8b31" />

---

## 💡 Lessons Learned
- Always back up the raw table before cleaning.
- Create new columns (`AgeClean`, `GenderClean`, etc.) — don’t overwrite originals.
- Functions (`fn_WordsToNumber`) are powerful for text → number conversions.
- Also ProperCase that help capitized first words and make the rest lower case.
- TRY_CAST are lifesavers for validating data.
- Documenting the process helps me see my mistakes and growth.

---







