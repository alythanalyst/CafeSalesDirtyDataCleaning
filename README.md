# Cafe Sales Data Cleaning Project

## Description

This project focused on cleaning a dataset of cafe sales records, "dirty_cafe_sales.csv," which contained approximately 10,000 records with significant missing and corrupted values. The goal was to prepare the data for analysis by addressing these issues and creating a clean, usable dataset.

## Data Source

* `dirty_cafe_sales.csv` (located in the `raw_data` folder)

## Cleaning Steps

1.  **Corrupted Value Handling:**
    * Replaced "UNKNOWN" and "ERROR" string values with null values to standardize missing data representation.
2.  **Missing Value Analysis:**
    * Identified and quantified missing values in each column:
        * Item: 969 missing values
        * Quantity: 479 missing values
        * Price Per Unit: 533 missing values
        * Total Spent: 502 missing values
        * Payment Method: 3178 missing values
        * Location (Instore, Takeaway): 3961 missing values
        * Transaction Date: 460 missing values
3.  **Missing Value Imputation/Removal:**
    * **Item and Price Per Unit:**
        * Extracted unique item names.
        * Used VLOOKUP to fill missing "Price Per Unit" values based on item names.
        * Resolved the ambiguity with "Juice" and "Cake" by counting sales quantities and applying VLOOKUP.
        * Filled remaining "Item" values using VLOOKUP based on price, then manually split remaining entries between "Cake" and "Juice".
    * **Quantity:**
        * Filled missing "Quantity" values by dividing "Total Spent" by "Price Per Unit."
    * **Total Spent:**
        * Filled missing "Total Spent" values by multiplying "Quantity" by "Price Per Unit."
    * **Payment Method:**
        * Calculated the percentage distribution of existing payment methods (Digital Wallet: 34%, Cash: 33%, Credit Card: 33%).
        * Filled missing "Payment Method" values based on these percentages.
    * **Location (Instore, Takeaway):**
        * Calculated the percentage distribution of existing locations.
        * Filled missing "Location" values based on these percentages.
    * **Transaction Date:**
        * Divided the year (2023) into 12 monthly sections.
        * Filled missing dates with random dates within their respective monthly sections.
4.  **Data Type Validation:**
    * Ensured that columns had appropriate data types (e.g., numeric for quantity and price, date for transaction date).
5.  **Data Consistency:**
    * Manually reviewed and addressed inconsistencies.
    * Removed 7 records: 3 fully null, and 4 with only one cell filled.
    * Manually corrected one record with a missing "Price Per Unit" and "Quantity" by deducing the values from the "Total Spent" (5 x 5).

## Tools Used

* Microsoft Excel

## Output

* `cleaned_cafe_sales.csv` (located in the `cleaned_data` folder)

## Documentation

* Detailed cleaning steps are documented in `documentation/cleaning_process.md`.

## Challenges

* High number of missing values, particularly in "Location" and "Payment Method."
* Resolving ambiguity between "Juice" and "Cake" prices.
* Handling missing dates with limited information.
* Deciding how to handle records with very few populated cells.
