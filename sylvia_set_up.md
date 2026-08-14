START
LOAD market price records
CREATE empty list for valid records
CREATE empty list for invalid records
FOR each record in dataset
    READ record ID, market, commodity, unit, price and quantity
    STANDARDIZE approved alternatives
        wakulima → Wakulima
        Nakuru top market → Nakuru Top Market
        KG, Kgs, kilogram → kg
        90 kg bag → 90kg bag

        
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
