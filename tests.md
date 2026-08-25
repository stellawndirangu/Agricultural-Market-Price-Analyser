# Agricultural Market-Price Analyser — Test Documentation

## Test 1: Normal Case — Analyse an Existing Commodity

**Purpose:** To verify that the program correctly analyses a valid commodity.

**Input:**

* Menu option: `3`
* Commodity: `Maize`

**Expected Result:**
The program should display the minimum, maximum and average price per kilogram for Maize, identify the cheapest and most expensive markets, count records above and below the average, and display the records from lowest to highest price.

**Actual Result:**
```
--- ANALYSIS: maize ---
Minimum price : KES 48.0/kg
Maximum price : KES 62.0/kg
Average price : KES 54.5/kg
Cheapest market       : Wakulima
Most expensive market : Kibuye
Records above average : 3
Records below average : 3
```

**Status:** PASS

---

## Test 2: Invalid Record

**Purpose:** To verify that the program identifies and rejects records that violate the validation rules.

**Input:**
Select menu option `5` to view invalid records.

**Expected Result:**
Records containing invalid values, such as a negative price, unsupported unit, or negative quantity, should be rejected and displayed together with the appropriate rejection reason.

**Actual Result:**
```
--- INVALID RECORDS ---
Record ID   Market              Commodity   Unit      Reason                   
-------------------------------------------------------------------------------
M023        Kibuye              Tomatoes    kg        Invalid Price            
M024        Kongowea            Potatoes    crate     Unsupported Unit         
M025        Gikomba             Maize       kg        Invalid Quantity         

Total invalid records: 3
```

**Status:** PASS

---

## Test 3: Boundary Case — Zero Quantity

**Purpose:** To verify that the program correctly handles the minimum acceptable quantity.

**Input:**  
A valid test record with `quantity_available = 0`.
We run this temporary cell to test:
```
test_record = {
    "record_id": "TEST001",
    "market": "Wakulima",
    "county": "Nairobi",
    "commodity": "Maize",
    "unit": "kg",
    "price_kes": 50,
    "quantity_available": 0
}

print(validate_record(test_record))
```

**Expected Result:**  
The record should be accepted because the validation rule allows quantity to be greater than or equal to zero.

**Actual Result:**  
The `validate_record()` function returned `(True, None)`, confirming that a quantity of zero is accepted as valid.

**Status:** PASS

---

## Test 4: Search Case — Commodity Not Found

**Purpose:** To verify that the program handles a search for a commodity that does not exist.

**Input:**

* Menu option: `3`
* Commodity: `Mangoes` 

**Expected Result:**
The program should display an appropriate message indicating that the commodity was not found and should not terminate unexpectedly.

**Actual Result:**
```
Enter selection:  3

Enter commodity to analyse:  Mangoes
No valid records found for "Mangoes".
```

**Status:** PASS

---

## Test 5: Invalid Menu Selection

**Purpose:** To verify that the menu handles an invalid selection correctly.

**Input:**

`9`

**Expected Result:**
The program should display an invalid-selection message and return to the main menu instead of terminating.

**Actual Result:**
```
Enter selection:  9

Invalid selection. Please try again.
```

**Status:** PASS

---

## Test 6: Market Comparison

**Purpose:** To verify that the program correctly compares a commodity between two valid markets.

**Input:**

* Menu option: `4`
* Select a valid commodity.
* Select two different markets where the commodity is available.

**Expected Result:**
The program should display the comparable price per kilogram for both markets, calculate the price difference, and identify the cheaper market.

**Actual Result:**
```
Enter selection:  4

Enter commodity to analyse market:  Tomatoes

Available markets:
- Gikomba
- Kibuye
- Kongowea
- Nakuru Top Market
- Wakulima

Enter first market:  gikomba
Enter second market:  wakulima

--- PRICE COMPARISON: Tomatoes ---
Average price in Gikomba: KES 96.00/kg
Average price in Wakulima: KES 92.00/kg
Price difference: KES 4.00/kg
Wakulima has a lower average price for Tomatoes (cheaper by KES 4.00/kg).
```
**Status:** PASS

---

## Test 7: Unit Conversion — 90kg Bag to Price per Kilogram

**Purpose:**  
To verify that the program correctly converts prices quoted per 90kg bag into prices per kilogram.

**Input:**  
- Menu option: `2`
- Commodity: `Maize`

**Expected Result:**  
For Maize records whose unit is `90kg bag`, the program should calculate the price per kilogram using:

`price_per_kg = price_kes / 90`

For example, the Wakulima price of KES 4,320 per 90kg bag should be:

`4320 / 90 = 48.0 KES/kg`

**Actual Result:**  
```
Enter selection:  2

Enter commodity:  Maize

--- Maize PRICES (lowest to highest) ---
Market              County    Unit      Price(KES)  KES/kg  
------------------------------------------------------------
Wakulima            Nairobi   90kg bag  4320        48.0    
Wakulima            Nairobi   kg        51          51      
Gikomba             Nairobi   kg        52          52      
Kongowea            Mombasa   90kg bag  4950        55.0    
Nakuru Top Market   Nakuru    kg        59          59      
Kibuye              Kisumu    90kg bag  5580        62.0  
```
The displayed conversions matched the expected calculations.

**Status:** PASS
## Test 8: Exit Option

**Purpose:** To verify that the program terminates correctly when the user selects Exit.

**Input:**

`7`

**Expected Result:**
The program should display the closing message and terminate normally.

**Actual Result:**
```
Enter selection:  7

Program closed.
```
**Status:** PASS
