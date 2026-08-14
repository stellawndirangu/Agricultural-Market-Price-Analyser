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
    VALIDATE record
    IF record ID is missing
        ADD record to invalid records with reason "Missing Record ID"
    ELSE IF market is missing
        ADD record to invalid records with reason "Missing Market"
    ELSE IF commodity is missing
        ADD record to invalid records with reason "Missing Commodity"
    ELSE IF unit is not kg or 90kg bag
        ADD record to invalid records with reason "Unsupported Unit"
    ELSE IF price is not numeric OR price <= 0
        ADD record to invalid records with reason "Invalid Price"
    ELSE IF quantity is not numeric OR quantity < 0
        ADD record to invalid records with reason "Invalid Quantity"
    ELSE
        IF unit = "kg"
            SET price_per_kg = price
            SET quantity_kg = quantity
        ELSE IF unit = "90kg bag"
            SET price_per_kg = price / 90
            SET quantity_kg = quantity * 90
        END IF
        STORE price_per_kg and quantity_kg in record
        ADD record to valid records
    END IF
END FOR
RETURN valid_records, invalid_records
END
