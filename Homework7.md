##### Q1. Write a query to grab all of the EV records in the EVRegistry table.

SELECT ModelYear, Make, Model

FROM EVRegistry 

##### Q2. Using the EVRegistry table, Write a query that lists all of the unique types of EV's. your reult set should have one column, ElectricVehicleType.

SELECT DISTINCT ElectricVehicleType 

FROM EVRegistry;

##### Q3. Using the EVRegistry, Write a query that shows all of the information on Battery Electric Vehicles (BEV) that are in the registry.

SELECT * FROM EVRegistry

WHERE ElectricVehicleType = "Battery Electric Vehicle (BEV)";

##### Q4. Using the EVRegistry, wirte a query that returns the Make and Model of all of the EV's that have a BaseMSRP between 20000 and 35000?

SELECT Make, Model 
FROM EVRegistry
WHERE BaseMSRP > 20000 and BaseMSRP < 35000;

### 7.2 HW Questions

##### Q1. Using EVRegistry, write a query to find a record where the City attribute is NULL. Return all of the available columns.

SELECT *
FROM EVRegistry
WHERE City IS NULL;

##### Q2. Write a query to find the make, model, and ElectricVehicleType where the VIN number has that ends in '3E1EA1J'.

SELECT Make, Model, ElectricVehicleType
FROM EVRegistry
WHERE VIN like "%3E1EA1J";

##### Q3. Select the ModelYear, make, model, ElectricVehicleType, and range of the Tesla vehicles or cheverolet vehicles in the registry. Order the result set by Make and Model year in from newest to oldest.

SELECT ModelYear, Make, Model, ElectricVehicleType, ElectricRange
FROM EVRegistry
WHERE Make = "TESLA" or  Make = "CHEVROLET"
ORDER BY Make, ModelYear DESC;

#### Q4. Using EVCharging, Write a query to find out how many many times those stations were used. Order them by the most used to the least used and limit the output to 5 records.

SELECT stationId, COUNT(*) as Uses 
FROM EVCharging
GROUP by stationId
ORDER by Uses DESC
LIMIT 5;

#### Q5. Using EVCharging, For the folks who charged longer than 0.5 hours, show the min and max of the charging time for each user. Your output columns should be userid, minTime, and maxTime. Order this result set by the last two columns respectively.

SELECT DISTINCT userId, MIN(chargeTimeHrs) as minHours, MAX(chargeTimeHrs) as maxHours
FROM EVCharging
Group by userId
Having Sum (ChargeTimeHrs) > 0.5
ORDER BY  minHours, maxHours;

### 7.3 Homework Questions


##### Q1. Using EVCharging, Which day of the week has the highest average charging time? Round the answer to 2 decimal points.

SELECT weekday, round(AVG(chargeTimeHrs),2) as avgChargeTime 
FROM EVCharging
Group by weekday
ORDER by Avg(chargeTimeHrs) DESC
LIMIT 1;


##### Q2. Using, EV charging, Find the total power consumed from charging EV's by each User. Return the `userId` and name the calculated column, `totalPower`. Round the answer to 2 deciaml points and list the out put in highest to lowest order. Limit the order to the top 15 users.

SELECT userId, round(AVG(kwhTotal),2) as totalPower 
FROM EVCharging
GROUP BY userId
ORDER BY totalPower DESC
LIMIT 15;


##### Q3. Using dimfacility and factCharge, write a query to find out which type of facility (GROUP BY) has the most amount of charging stations. Return `type Facility` and name the calculated column `numStation`. Order the result set from highest to lowest number of charging stations.

SELECT df.typeFacility as 'Type of Facility', count(fc.stationId) as 'Number of Stations'
FROM dimFacility as df
INNER JOIN factCharge as fc
ON df.FacilityKey = fc.facilityID
GROUP BY df.typeFacility
ORDER BY count(fc.stationId) DESC;


##### Q4. In your own words, Briefly explain Primary Keys and Foreign Keys.

Primary Keys are like speacial ID cards for each row in a database table that uniquely identifies each record that exist. 

Foreign Key is like a link between two tables. It refers to a primary key in a connected table


##### Q5. Using EV Charging, For the folks who charged longer than one hour, show the min and max of the charging time for each user. Your output columns should be `userid`, `minTime`, and `maxTime`. Order this result set by the last two columns respectively. HINT: USE `HAVING`

SELECT userId, MIN(chargeTimeHrs) as minTime, MAX(chargeTimeHrs) as maxTime
FROM EVCharging
GROUP BY userId
HAVING chargeTimeHrs > 1
ORDER BY 2 DESC, 3 DESC;