# S&P 500 Index Fund OHLCV (2016-2017)

## Known for its risk profile, S&P 500 is an index of top performing stocks. The following 2 databases included the open-high-low-close-volume data from 2016 through 2017. Let's see what the data tell us.

1. upload the databases
```
SELECT * FROM S_P  
# this gives us 2016 records
```

```
SELECT * FROM SPIndex
# return 2016-2017 partial data
```

2. How many entries are there and what is the average open value?
```
SELECT COUNT(DISTINCT Date), AVG(Open)
FROM S_P
# as many as 412 rows/days and an opening of 2086 on average
```
3. Which date(s) has larger open than close values?
```
SELECT Date
FROM S_P
GROUP BY Date
HAVING AVG(Open) > (SELECT AVG(Close) 
                    from S_P)
Order by Date DESC

Results:

Date
9/30/16
9/29/16
9/26/16
8/9/16
8/8/16
8/5/16
8/4/16
```
4. On which day does it see the highest trading volume?

```
# volume indicates total number of shares traded. Larger value is beneficial to stock appreciation
# let's find out when the stocks are most appealing to buyers

SELECT volume, max(date) as latest_date
FROM SPIndex
GROUP BY volume
ORDER BY latest_date ASC

# turns out 1/10/17 has the largest volume at 3638790000
```

5. How to combine 2 tables and have a complete 2016-2017 record?
```
SELECT * FROM S_P
UNION
SELECT * FROM SPIndex
GROUP BY c1

# we now have all records from 2 tables
```

6. What are the matching dates in 2 tables?
```
# A JOIN clause will outline a conjunction

SELECT t1.c1
FROM S_P as t1
INNER JOIN SPIndex as t2
ON t1.c1 = t2.c1

Results:

Date
10/3/16
10/4/16
10/5/16
10/6/16
......
12/28/16
12/29/16
12/30/16

# October to December records exist in both databases
```
7. What are the unique values from SPIndex?
```
# That is, to return 2017 records by RIGHT JOIN SPIndex

SELECT t2.c1, t2.c2, t2.c5
FROM S_P as t1
RIGHT JOIN SPIndex as t2
ON t1.c1 = t2.c1
WHERE t1.c1 is NULL

Results:

Date	  Open	      Close
1/3/17	2251.570068	2257.830078
1/4/17	2261.600098	2270.75
1/5/17	2268.179932	2269
1/6/17	2271.139893	2276.97998
......
11/30/17	2633.929932	2647.580078
12/1/17	2645.100098	2642.219971
12/4/17	2657.189941	2639.439941
12/5/17	2639.780029	2629.570068
```

8. What is the total trading volume of 2017?

```
SELECT t2.c1, SUM(t2.c6)
FROM S_P as t1
RIGHT JOIN SPIndex as t2
ON t1.c1 = t2.c1
WHERE t1.c1 is NULL

# Result: 802741450000
```
