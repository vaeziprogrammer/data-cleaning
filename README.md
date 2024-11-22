# world life expectancy project (data cleaning)

SELECT * 
FROM world_Life_expectency.WorldLifeExpectancy
;

#first, let's find duplicate values in our rows 

SELECT Country, Year, CONCAT(country,' ', year), COUNT(CONCAT(country,' ', year))
FROM WorldLifeExpectancy
GROUP BY Country, Year, CONCAT(Country, year)
HAVING COUNT(CONCAT(Country, year)) > 1
;

# This is a simplified version:

SELECT Country, Year, COUNT(*) AS DuplicateCount
FROM WorldLifeExpectancy
GROUP BY Country, Year
HAVING COUNT(*) > 1;


# This code is specifically designed to identify duplicate rows in our WorldLifeExpectancy dataset based on the combination of Country and Year.
SELECT *
FROM (
	SELECT Row_ID,
    CONCAT(Country, Year),
    ROW_NUMBER() OVER( PARTITION BY CONCAT(Country, Year) ORDER BY CONCAT(Country, Year)) as Row_Num
    FROM WorldLifeExpectancy
    ) AS Row_Table 
WHERE Row_Num > 1
;

# We found 3 duplicate rows in our dataset, let's delete this

DELETE FROM WorldLifeExpectancy
WHERE Row_ID IN (
    SELECT Row_ID FROM (
        SELECT Row_ID,
               ROW_NUMBER() OVER (PARTITION BY CONCAT(Country, Year) ORDER BY CONCAT(Country, Year)) AS Row_Num
        FROM WorldLifeExpectancy
    ) AS Row_Table
    WHERE Row_Num > 1
);

# We need to turn off SQL safe mode update.
SET SQL_SAFE_UPDATES = 0;









  




