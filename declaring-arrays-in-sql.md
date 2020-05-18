# Declaring Arrays in SQL

Sometimes I get a ticket to update/delete data across multiple tables for some entities with given IDs. For example:

```
UPDATE breeder
SET is_a_client = false
WHERE breeder_id IN ('A', 'B');

UPDATE puppy
SET is_for_sale = false
WHERE breeder_id IN ('A', 'B');

UPDATE puppy_sale
SET transaction = 'void'
WHERE puppy_id IN (
    SELECT puppy_id FROM puppy WHERE breeder_id IN ('A', 'B')
);
```
Notice that `WHERE breeder_id IN ('A', 'B')` is repeated multiple times. This is probably fine for this volume but imagine even just 3 more tables need to get updated. Performing queries like this are very prone to human-error.

***

### PostgreSQL

```
DO $$
    DECLARE breederIdArray string ARRAY;

BEGIN
    SELECT array_agg(breeder_id)
    INTO breederIdArray
    FROM breeder
    WHERE breeder_id in ('A', 'B');

    UPDATE puppy
    SET is_for_sale = false
    WHERE breeder_id = ANY(breederIdArray);

    UPDATE puppy_sale
    SET transaction = 'void'
    WHERE puppy_id IN (
        SELECT puppy_id FROM puppy WHERE breeder_id IN ('A', 'B')
    );

END $$;
```

Note you can also create temporary tables and select values into those - see [this S/O post](https://stackoverflow.com/questions/15691243/creating-temporary-tables-in-sql). Be sure to drop the temp tables at the end of your query.

***

### Microsoft SQL Server

```
DECLARE @BreederIdTable TABLE (id varchar(255));

INSERT INTO @BreederIdTable
SELECT BreederId FROM Breeder
WHERE BreederId in ('A', 'B');

Update Puppy
SET IsForSale = 0
WHERE BreederId IN (SELECT id FROM BreederIdTable);

Update PuppySale
SET Transaction = 'void'
WHERE PuppyId IN (
    SELECT PuppyId FROM Puppy WHERE BreederId IN (SELECT id FROM BreederIdTable)
);
```
