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
FUNCTION view_commodities(valid_records):
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
FUNCTION view_prices_for_commodity(valid_records):
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

## Step 9. Compare two markets
FUNCTION COMPARE_MARKETS():
    DISPLAY "Available commodities"

    GET list of unique commodities from valid records
    SORT commodities alphabetically

    DISPLAY commodities

    INPUT selected commodity

    IF selected commodity does not exist THEN
        DISPLAY "Invalid commodity"
        RETURN
    END IF


    GET all markets available for the selected commodity
    SORT markets alphabetically

    DISPLAY available markets

    INPUT first market

    IF first market does not exist THEN
        DISPLAY "Invalid first market"
        RETURN
    END IF

    INPUT second market

    IF second market does not exist THEN
        DISPLAY "Invalid second market"
        RETURN
    END IF

    IF first market = second market THEN
        DISPLAY "Please select two different markets"
        RETURN
    END IF


    FIND record for selected commodity in first market
    FIND record for selected commodity in second market

    IF first market record does not exist THEN
        DISPLAY "No valid record found for first market"
        RETURN
    END IF

    IF second market record does not exist THEN
        DISPLAY "No valid record found for second market"
        RETURN
    END IF


    IF first market unit = "kg" THEN
        first price per kg = first market price

    ELSE IF first market unit = "90kg bag" THEN
        first price per kg = first market price / 90
    END IF


    IF second market unit = "kg" THEN
        second price per kg = second market price

    ELSE IF second market unit = "90kg bag" THEN
        second price per kg = second market price / 90
    END IF


    CALCULATE price difference =
        ABS(first price per kg - second price per kg)


    IF first price per kg < second price per kg THEN
        cheaper market = first market

    ELSE IF second price per kg < first price per kg THEN
        cheaper market = second market

    ELSE
        cheaper market = "Both markets have the same price"
    END IF


    DISPLAY selected commodity
    DISPLAY first market and price per kg
    DISPLAY second market and price per kg
    DISPLAY price difference
    DISPLAY cheaper market

END FUNCTION


