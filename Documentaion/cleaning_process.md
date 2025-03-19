# Cafe Sales Data Cleaning Process

## Introduction

This document details the step-by-step process used to clean the "dirty_cafe_sales.csv" dataset. The dataset contained significant missing and corrupted values, requiring careful handling to prepare it for analysis.

## 1. Corrupted Value Handling

* **Action:** Replaced "UNKNOWN" and "ERROR" string values with null values.
* **Method:** Used Excel's "Find and Replace" functionality.
    * Find: "UNKNOWN"
    * Replace: (left blank)
    * Repeat for "ERROR".
* **Rationale:** Standardized the representation of missing values, making them easier to identify and handle.

## 2. Missing Value Analysis

* **Action:** Identified and quantified missing values in each column.
* **Method:** Used Excel's `COUNTBLANK()` function for each column.
* **Results:**
    * Item: 969 missing values
    * Quantity: 479 missing values
    * Price Per Unit: 533 missing values
    * Total Spent: 502 missing values
    * Payment Method: 3178 missing values
    * Location (Instore, Takeaway): 3961 missing values
    * Transaction Date: 460 missing values

## 3. Missing Value Imputation/Removal

### 3.1. Item and Price Per Unit

* **Action:** Imputed missing "Item" and "Price Per Unit" values.
* **Method:**
    1.  **Unique Item Extraction:** Created a list of unique item names using Excel's "Remove Duplicates" feature.
    2.  **Price Lookup:** Used `VLOOKUP()` to populate missing "Price Per Unit" values based on item names.
        * Example formula: `=VLOOKUP(B2,UniqueItemsPriceList!A:B,2,FALSE)` (assuming "Item" is in column B and unique items/prices are in a separate sheet).
    3.  **Juice/Cake Resolution:**
        * Noticed "Juice" and "Cake" had the same price.
        * Counted sales of each using `COUNTIF()`.
        * Confirmed similar sales quantities.
        * Applied `VLOOKUP()` to fill prices, excluding "Juice" and "Cake".
        * Used `VLOOKUP()` based on price to fill most "Item" names.
        * Manually split remaining items between "Cake" and "Juice" (approximately 50/50).
* **Rationale:** Leveraged existing data relationships to fill missing values. The manual split was a practical solution to the ambiguity.

### 3.2. Quantity

* **Action:** Imputed missing "Quantity" values.
* **Method:** Divided "Total Spent" by "Price Per Unit".
    * Formula: `=IF(ISBLANK(C2),IF(AND(ISNUMBER(E2),ISNUMBER(D2)),E2/D2,C2),C2)` (assuming "Quantity" is in C, "Price Per Unit" in D, and "Total Spent" in E).
* **Rationale:** Used a direct calculation based on existing related data.

### 3.3. Total Spent

* **Action:** Imputed missing "Total Spent" values.
* **Method:** Multiplied "Quantity" by "Price Per Unit".
    * Formula: `=IF(ISBLANK(E2),IF(AND(ISNUMBER(C2),ISNUMBER(D2)),C2*D2,E2),E2)`
* **Rationale:** Used a direct calculation based on existing related data.

### 3.4. Payment Method

* **Action:** Imputed missing "Payment Method" values.
* **Method:**
    1.  Calculated the percentage distribution of existing payment methods using `COUNTIF()` and dividing by the total count.
    2.  Observed approximately equal percentages (Digital Wallet: 34%, Cash: 33%, Credit Card: 33%).
    3.  Used Excel's `RAND()` and `IF()` functions to assign payment methods based on these percentages.
* **Rationale:** Assigned values based on the observed distribution, introducing randomness to avoid bias.

### 3.5. Location (Instore, Takeaway)

* **Action:** Imputed missing "Location" values.
* **Method:**
    1. Calculated the percentage distribution of existing locations.
    2. Observed similar percentages.
    3. Used `RAND()` and `IF()` to assign locations based on these percentages.
* **Rationale:** Similar to payment method imputation, used the observed distribution to fill missing values.

### 3.6. Transaction Date

* **Action:** Imputed missing "Transaction Date" values.
* **Method:**
    1.  Divided the year (2023) into 12 monthly sections.
    2.  Used `RANDBETWEEN()` to generate random dates within each section's respective month.
    3. Used If statements to detect the null value and insert the random date from the correct month.
* **Rationale:** Introduced randomness within monthly constraints due to the lack of other information.

### 3.7. Record Removal

* **Action:** Removed 7 records.
* **Method:** Manually deleted the rows.
* **Rationale:** 3 records were completely null. 4 records had only one cell populated, making meaningful imputation impossible.

### 3.8. Manual Correction

* **Action:** Manually corrected one record with missing "Price Per Unit" and "Quantity".
* **Method:** Deduced the values from "Total Spent" (25) and determined the only logical combination was 5 x 5.
* **Rationale:** Used logical deduction to correct an inconsistency.

## 4. Data Type Validation

* **Action:** Ensured columns had appropriate data types.
* **Method:** Used Excel's "Format Cells" option to set data types (e.g., numeric, date).
* **Rationale:** Ensured data consistency and compatibility for analysis.

## 5. Data Consistency

* **Action:** Manually reviewed and addressed inconsistencies.
* **Method:** Visual inspection and manual correction.
* **Rationale:** Ensured data accuracy and reliability.

## Conclusion

The "dirty_cafe_sales.csv" dataset was successfully cleaned by addressing missing and corrupted values using a combination of automated and manual methods. The cleaned data is now ready for further analysis.