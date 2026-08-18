```
# Step 1 Loading the data
FUNCTION LOAD_DATA()
    RETURN hardcoded list of market price records
END FUNCTION


# Step 2 Standardizing the data
FUNCTION STANDARDIZE_RECORD(record)
    IF market = "wakulima"
        SET market = "Wakulima"
    IF market = "Nakuru top market"
        SET market = "Nakuru Top Market"
    IF unit = "KG" OR unit = "Kgs" OR unit = "kilogram"
        SET unit = "kg"
    IF unit = "90 kg bag"
        SET unit = "90kg bag"
    RETURN record
END FUNCTION


# Step 3 Validating Records
FUNCTION VALIDATE_RECORD(record)
    IF record ID is missing
        RETURN False, "Missing Record ID"
    ELSE IF market is missing
        RETURN False, "Missing Market"
    ELSE IF commodity is missing
        RETURN False, "Missing Commodity"
    ELSE IF unit is not "kg" or "90kg bag"
        RETURN False, "Unsupported Unit"
    ELSE IF price is not numeric OR price <= 0
        RETURN False, "Invalid Price"
    ELSE IF quantity is not numeric OR quantity < 0
        RETURN False, "Invalid Quantity"
    ELSE
        RETURN True, None
    END IF
END FUNCTION


# Step 4 Derived Values
FUNCTION CALCULATE_DERIVED_FIELDS(record)
    IF unit = "kg"
        SET price_per_kg = price
        SET quantity_kg = quantity
    ELSE IF unit = "90kg bag"
        SET price_per_kg = price / 90
        SET quantity_kg = quantity * 90
    END IF
    STORE price_per_kg and quantity_kg in record
    RETURN record
END FUNCTION


# Step 5 Main Program flow
FUNCTION PREPARE_RECORDS()
    raw_records ← LOAD_DATA()
    CREATE empty list for valid records
    CREATE empty list for invalid records

    FOR each record in raw_records
        record ← STANDARDIZE_RECORD(record)
        is_valid, reason ← VALIDATE_RECORD(record)

        IF is_valid
            record ← CALCULATE_DERIVED_FIELDS(record)
            ADD record to valid records
        ELSE
            STORE reason in record as rejection_reason
            ADD record to invalid records
        END IF
    END FOR

    RETURN valid records, invalid records
END FUNCTION

#Step 6 View Available Commodities
    IF valid_records is empty THEN
        PRINT "No valid records available."
        RETURN
    END IF

    CREATE empty set commodities

    FOR EACH record IN valid_records
        ADD record.commodity TO commodities
    END FOR

    SET sorted_commodities = SORT(commodities) ALPHABETICALLY

    PRINT "Available commodities:"

    FOR EACH commodity IN sorted_commodities
        PRINT " - " + commodity
    END FOR
END FUNCTION

#Step 7 View Prices for a Commodity
IF valid_records is empty:
    PRINT "No valid records available."
    RETURN

CALL view_commodities(valid_records)

commodity = INPUT "Enter commodity: "
commodity = REMOVE leading and trailing spaces from commodity

selected_records = empty list

FOR each record in valid_records:

    IF record["commodity"] matches commodity:
        ADD record to selected_records

IF selected_records is empty:
    PRINT "Commodity not found."
    RETURN

FOR each record in selected_records:

    IF record["unit"] is "kg":
        price_per_kg = record["price_kes"]

    ELSE IF record["unit"] is "90kg bag":
        price_per_kg = record["price_kes"] / 90

    DISPLAY market,county, unit, price and price_per_kg

SORT selected_records by price_per_kg from lowest to highest

PRINT "Prices for selected commodity:"

END FUNCTION


## Step 8. Analyse a commodity
READ commodity name
matches = [r for r in valid_records if r.commodity == name]
min_price  = MIN(matches.price_per_kg)
max_price  = MAX(matches.price_per_kg)
avg_price  = AVERAGE(matches.price_per_kg)
cheapest_market  = market of record with min_price
priciest_market  = market of record with max_price
above_avg_count  = COUNT(r.price_per_kg > avg_price)
below_avg_count  = COUNT(r.price_per_kg < avg_price)
DISPLAY min, max, avg, cheapest_market, priciest_market,
        above_avg_count, below_avg_count
DISPLAY matches SORTED lowest to highest price

