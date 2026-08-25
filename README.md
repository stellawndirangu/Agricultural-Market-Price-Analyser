# Agricultural Market-Price Analyser

## Problem Statement

Farmers and traders need a simple way to compare agricultural commodity prices across different markets. The dataset contains market-price records for several commodities, with prices recorded either per kilogram or per 90-kilogram bag. The dataset also contains deliberate data-quality issues such as inconsistent capitalization, unsupported units, negative values, and other invalid entries.

This project develops a menu-driven Python program that validates, standardizes, processes, and analyses agricultural market-price records using core Python programming concepts.

## Project Objectives

The objectives of the program are to:

* Store market-price records using an appropriate Python data structure.
* Standardize only the recognized market and unit variations provided in the project instructions.
* Validate every record before analysis.
* Reject invalid records and report the reason for rejection.
* Convert all prices into a common price-per-kilogram measure.
* Convert quantities recorded in 90-kilogram bags into kilograms.
* Display unique commodities in alphabetical order.
* Display market prices for a selected commodity.
* Calculate the minimum, maximum, and average price per kilogram for a selected commodity.
* Identify the cheapest and most expensive markets for a commodity.
* Count records with prices above and below the average.
* Sort market-price records from the lowest to the highest price.
* Compare the price of a selected commodity between two markets.
* Display invalid records and a dataset summary.

## Programming Approach

The project uses core Python only. No Pandas, NumPy, machine-learning libraries, or charting libraries are used.

The market-price data is stored as a **list of dictionaries**. Each dictionary represents one market-price record and contains fields such as:

* Record ID
* Market
* County
* Commodity
* Unit
* Price
* Quantity available

The program is divided into separate functions so that each function performs a specific task.

## Main Functions

### `load_data()`

Loads and returns the raw market-price records used by the program.

### `standardize_record(record)`

Standardizes only the recognized alternative values defined in the project instructions.

Examples include:

* `wakulima` → `Wakulima`
* `Nakuru top market` → `Nakuru Top Market`
* `KG`, `Kgs`, `kilogram` → `kg`
* `90 kg bag` → `90kg bag`

Unsupported values are not guessed or automatically corrected.

### `validate_record(record)`

Checks whether each record satisfies the required validation rules.

The function returns whether the record is valid and, where necessary, the reason for rejection.

### `calculate_derived_fields(record)`

Converts valid records into common units.

For records measured in kilograms:

`price_per_kg = price`

`quantity_kg = quantity`

For records measured in 90-kilogram bags:

`price_per_kg = price / 90`

`quantity_kg = quantity × 90`

### `prepare_records()`

Processes all raw records by:

1. Standardizing recognized values.
2. Validating each record.
3. Calculating derived values for valid records.
4. Separating valid and invalid records.

### `view_commodities()`

Displays the unique commodities contained in the valid dataset in alphabetical order.

### `view_prices_for_commodity()`

Allows the user to select a commodity and displays its valid market-price records, sorted from the lowest to the highest price per kilogram.

### `analyse_commodity()`

Analyses a selected commodity and displays:

* Minimum price per kilogram
* Maximum price per kilogram
* Average price per kilogram
* Cheapest market
* Most expensive market
* Number of records above the average
* Number of records below the average
* Sorted market-price records

### `compare_markets()`

Allows the user to select one commodity and compare its price between two different markets. The function displays the prices, price difference, and cheaper market.

Where more than one valid record exists for the same commodity and market, the program uses the average price per kilogram for comparison.

### `view_invalid_records()`

Displays records that failed validation together with their rejection reasons.

### `data_summary()`

Displays a summary of the dataset, including:

* Total number of records
* Number of valid records
* Number of invalid records
* Total valid quantity converted to kilograms
* Number of records originally recorded in kilograms
* Number of records originally recorded as 90-kilogram bags

## Validation Rules

A record is considered valid only when:

* Record ID is provided.
* Market is provided.
* Commodity is provided.
* Unit resolves to either `kg` or `90kg bag`.
* Price is numeric and greater than zero.
* Quantity is numeric and not negative.

Records that fail any of these checks are rejected and stored together with the reason for rejection.

## Menu Options

The program provides the following menu:

1. View available commodities
2. View prices for a commodity
3. Analyse a commodity
4. Compare two markets
5. View invalid records
6. View dataset summary
7. Exit

The menu repeats until the user selects Exit. Invalid menu selections display an error message and return the user to the menu.

## How to Run the Program

1. Ensure Python is installed on the computer.
2. Save the Python source file as `main.py`.
3. Open a terminal or command prompt.
4. Navigate to the folder containing `main.py`.
5. Run the program using:

```bash
python main.py
```

6. Select an option from the displayed menu.
7. Follow the prompts shown by the program.
8. Select option `7` to close the program.

## Project Requirements

This project demonstrates the use of:

* Algorithms
* Variables
* Conditional statements
* Loops
* Lists
* Dictionaries
* Sets
* Functions
* Input validation
* Data standardization
* Testing
* Documentation
* Git version control

## Authors — Group 3

| Admission Number | Name                    |
| ---------------- | ----------------------- |
| 60770            | Stella Wanjiku Ndirangu |
| 230510           | Benson Ongocho          |
| 225879           | Lorna Koskey            |
| 227942           | Duncan Muema            |
| 230701           | Sylvia Wambui Njau             |

