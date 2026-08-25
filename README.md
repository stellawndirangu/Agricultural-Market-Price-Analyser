# Agricultural-Market-Price-Analyser
This is a menu-driven Python program for comparing agricultural commodity prices across Kenyan markets. The project was developed for Project 3 of the MSc Fundamentals of Programming Capstone Project and focuses on core Python programming concepts such as variables, decisions, loops, data structures, functions, validation, testing, documentation and Git.

## Problem Definition

Farmers and traders need to compare the prices of agricultural commodities across different markets, but price records may use different units and may contain incomplete, inconsistent or invalid values. In this project, the program processes market-price records for four commodities across five markets. Prices can be recorded either per kilogram (kg) or per 90-kilogram bag (90kg bag).

The Agricultural Market-Price Analyser addresses this problem by validating market records, standardising recognised variations, converting prices and quantities to a common kilogram basis, and providing a menu-driven interface for viewing and comparing prices. Invalid records are rejected and reported with a specific reason rather than being silently corrected.

## Project Objectives

The main objectives of the project are to:

- Store agricultural market-price records using an appropriate Python data structure.

- Validate each record and identify invalid data with clear rejection reasons.

- Standardise only the recognised market and unit alternatives defined in the project brief.

- Calculate a comparable price per kilogram for records supplied in either kilograms or 90kg bags.

- Convert quantities supplied as 90kg bags into kilograms.

- Display the unique commodities in alphabetical order.

- Analyse a selected commodity by calculating its minimum, maximum and average price per kilogram.

- Identify the cheapest and most expensive markets for a selected commodity.

- Compare the price of a selected commodity between two markets.

- Provide record-quality and quantity summaries through a simple menu-driven interface.

## Dataset

The program works with the 03_Market_Prices dataset specified in the capstone exercise.

The records used by the current implementation contain the following fields:

Field                                 Description

record_id -                            Unique identifier for the market-price record

market -                               Name of the agricultural market

county -                               County where the market is located

commodity -                            Agricultural commodity

unit -                                 Price unit: kg or 90kg bag

price_kes -                            Recorded price in Kenyan shillings

quantity_available -                   Quantity available in the stated unit

## Recognised Data Standardisation

The program standardises only the alternatives explicitly approved by the project brief.

Input variation                        Standard value

wakulima -                              Wakulima

Nakuru top market -                     Nakuru Top Market

KG -                                    kg

Kgs -                                   kg

kilogram -                              kg

90 kg bag -                             90kg bag

## Validation Rules

Every record is checked against the following rules before it is included in the valid-record collection.

Record ID

- Record ID must be provided.
- A missing Record ID causes the record to be rejected.

Market

- Market must be provided.
- Recognised market-name alternatives are standardised before validation.

Commodity

- Commodity must be provided.

Unit

- The unit must resolve to either:
  - kg
  - 90kg bag

- Unsupported units are rejected.

Price

- Price must be numeric.
- Price must be greater than zero.
- Zero and negative prices are rejected.

Quantity

- Quantity must be numeric.
- Quantity must not be negative.
- Negative quantities are rejected.

Invalid-record handling

Invalid records are not silently discarded. The program keeps them separately and records a rejection reason such as:

- Missing Record ID
- Missing Market
- Missing Commodity
- Unsupported Unit
- Invalid Price
- Invalid Quantity

The current implementation's validate_record() function returns a Boolean result together with the reason when a record fails validation. 

## Price and Quantity Calculations

To make prices comparable across records, the program converts all prices to a price-per-kilogram value.

Price per kilogram

For a record already priced per kilogram:

price_per_kg = price_kes

For a 90kg bag:

price_per_kg = price_kes / 90

Quantity in kilograms

For a record already expressed in kilograms:

quantity_kg = quantity_available

For a 90kg bag:

quantity_kg = quantity_available × 90

The implementation stores the derived values in each valid record through calculate_derived_fields().

## Main Functions

The program is divided into small functions so that individual operations can be developed, tested and maintained independently.

Function -                                  Purpose

load_data() -                               Loads the market-price records into a list of dictionaries.

standardize_record(record) -                Applies only the approved market and unit mappings.

validate_record(record) -                   Checks a single record against the validation rules and returns a rejection reason when invalid.

prepare_records() -                         Prepares records for processing by applying standardisation, validation and derived-field calculations.

calculate_derived_fields(record) -          Calculates price_per_kg and quantity_kg.

calculate_price_per_kg(record) -            Returns the price per kilogram based on the record's unit.

get_unique_sorted_values(records, field) -  Returns unique values for a field in sorted order.

get_matching_records(records, commodity_name) - Finds records matching a selected commodity.

sort_records_by_price(records)-             Sorts records from the lowest to highest price per kilogram.

print_table(rows, headers, widths) -        Displays records in a fixed-width table.

option_1_view_commodities() -               Displays available commodities alphabetically.

option_2_view_prices_for_commodity() -      Displays prices for a selected commodity from lowest to highest.

option_3_analyse_commodity() -              Calculates minimum, maximum and average price and identifies cheapest/most expensive markets.

compare_markets() -                         Compares a commodity's average price per kilogram between two markets.

option_4_compare_markets_input() -          Collects and validates the commodity and two markets selected by the user.

option_5_view_invalid_records() -           Displays rejected records and their rejection reasons.

option_6_display_dataset_summary() -        Displays total, valid and invalid records plus quantity summaries.

display_menu() -                            Displays the main program menu.

run_menu() -                                Controls the program using the required while loop, input(), if–elif–else and break.


## Program Menu

The program uses the required repeating menu:

=== AGRICULTURAL MARKET-PRICE ANALYSER ===

1. View available commodities
2. View prices for a commodity
3. Analyse a commodity
4. Compare two markets
5. View invalid records
6. View dataset summary
7. Exit

Each non-exit menu option calls a separate function. The menu continues running until the user selects option 7. An invalid menu selection displays an error message and returns to the menu.

## Menu Operations

Option 1 — View available commodities

Displays the unique valid commodities in alphabetical order.

Option 2 — View prices for a commodity

The user enters a commodity name. The program displays the available valid records for that commodity, sorted from the lowest to highest price per kilogram.

Option 3 — Analyse a commodity

For the selected commodity, the program calculates:

- Minimum price per kilogram
- Maximum price per kilogram
- Average price per kilogram
- Cheapest market
- Most expensive market
- Number of records above the average
- Number of records below the average

Option 4 — Compare two markets

The user selects:
- A commodity
- First market
- Second market

The program then reports the average price per kilogram in each market, the price difference, and which market is cheaper.

Option 5 — View invalid records

Displays rejected records together with their validation reasons.

Option 6 — View dataset summary

Displays:
- Total records
- Valid records
- Invalid records
- Total original quantity
- Total quantity converted to kilograms
- Number of kilogram records
- Number of 90kg-bag records

Option 7 — Exit

Terminates the program.

## Running the Project

Requirements
- Python 3.x
- Google Colab, Jupyter Notebook or another Python environment
- Git

The attached notebook was created with a Python 3 kernel.

Option A — Run the Jupyter Notebook

The current submitted implementation is provided as:

Group_3.ipynb

1. Clone or download the repository.
2. Open the project folder in Jupyter Notebook, JupyterLab or VS Code.
3. Open Group_3.ipynb.
4. Run the cells from top to bottom.
5. When the final menu cell runs, use the displayed menu to interact with the program.

The notebook currently contains the dataset and program logic directly in the notebook, so no external data-library installation is required.

Option B - Open file in Google Colab

The current submitted implementation is provided as:

Group_3.ipynb

1. Clone or download the repository.
2. Open Google colab on your browser
3. Open Group_3.ipynb.
4. Run the cells from top to bottom.
5. When the final menu cell runs, use the displayed menu to interact with the program.

## Data Quality Handling

The dataset deliberately contains records that demonstrate data-quality problems. The current implementation includes examples of:

- Recognised alternative spellings/capitalisation that can be standardised.
- A negative price.
- An unsupported unit.
- A negative quantity.

The program therefore demonstrates the required distinction between:

Correctable recognised variation and Unsupported or invalid data that must be rejected.

## GitHub Repository

Project repository:

Agricultural Market-Price Analyser

[View the project on GitHub](https://github.com/stellawndirangu/Agricultural-Market-Price-Analyser/tree/main)

# Authors

- 060770 - Stella Wanjiku Ndirangu
- 230510 - Benson Ongocho
- 225879 - Lorna Koskey
- 227942 - Duncan Muema
- 230701 - Sylvia Njau


MSc Fundamentals of Programming
Project 3 — Agricultural Market-Price Analyser
