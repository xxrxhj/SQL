# Finance: S&P 500 Index Fund OHLCV (2016-2017)

## S&P 500 index is a benchmark of the U.S. stock market. Databases included the open-high-low-close-volume data from 2016 to 2017. Written in SQL, what does the data tell us?

1. Prepare S_P and SPIndex databases
```
SELECT * FROM S_P  
# this gives us 2016 records
```

| Date     | Open        | High        | Low         | Close       | Volume     |
|----------|-------------|-------------|-------------|-------------|------------|
| 1/4/16   | 2038.199951 | 2038.199951 | 1989.680054 | 2012.660034 | 4304880000 |
| 1/5/16   | 2013.780029 | 2021.939941 | 2004.170044 | 2016.709961 | 3706620000 |
| 1/6/16   | 2011.709961 | 2011.709961 | 1979.050049 | 1990.26001  | 4336660000 |
| 1/7/16   | 1985.319946 | 1985.319946 | 1938.829956 | 1943.089966 | 5076590000 |
| ........ | ........... | ........... | ........... | ........... | .......... |
| 12/27/16 | 2266.22998  | 2273.820068 | 2266.149902 | 2268.879883 | 1987080000 |
| 12/28/16 | 2270.22998  | 2271.310059 | 2249.110107 | 2249.919922 | 2392360000 |
| 12/29/16 | 2249.5      | 2254.51001  | 2244.560059 | 2249.26001  | 2336370000 |
| 12/30/16 | 2251.610107 | 2253.580078 | 2233.620117 | 2238.830078 | 2670900000 |



```
SELECT * FROM SPIndex
# return 2016-2017 partial data
```
| Date     | Open       | High       | Low        | Close      | Volume     |
|----------|------------|------------|------------|------------|------------|
| 10/3/16  | 2164.33008 | 2164.40991 | 2154.77002 | 2161.19995 | 3137550000 |
| 10/4/16  | 2163.37012 | 2165.45996 | 2144.01001 | 2150.48999 | 3750890000 |
| 10/5/16  | 2155.24990 | 2163.94995 | 2155.14990 | 2159.72998 | 3906550000 |
| ........ | .......... | .......... | .......... | .......... | .......... |
| 11/29/17 | 2627.82007 | 2634.88989 | 2620.32007 | 2626.07007 | 4078280000 |
| 11/30/17 | 2633.92993 | 2657.73999 | 2633.92993 | 2647.58008 | 4938490000 |
| 12/1/17  | 2645.10010 | 2650.62012 | 2605.52002 | 2642.21997 | 3942320000 |
| 12/4/17  | 2657.18994 | 2665.18994 | 2639.03003 | 2639.43994 | 4023150000 |
| 12/5/17  | 2639.78003 | 2648.71997 | 2627.72998 | 2629.57007 | 3539040000 |

2. How many entries are there in S_P and what is the average open value?
```
SELECT COUNT(DISTINCT Date), AVG(Open)
FROM S_P
```
There are as many as 252 rows/days and an opening of 2086 on average

3. Which date(s) has higher open values compared to close?
```
SELECT Date
FROM S_P
GROUP BY Date
HAVING AVG(Open) > (SELECT AVG(Close) 
                    from S_P)
Order by Date DESC
```
Results:

| Date    |
|---------|
| 9/30/16 |
| 9/29/16 |
| 9/26/16 |
| 8/9/16  |
| 8/8/16  |
| 8/5/16  |
| 8/4/16  |

4. On which day does it see the largest trading volume?

```
# volume indicates total number of shares traded. Larger value is beneficial to stock appreciation
# let's find out when the stocks are most appealing to buyers

SELECT volume, max(date) as latest_date
FROM SPIndex
GROUP BY volume
ORDER BY latest_date ASC
```
- It turns out 1/10/17 has the largest volume at $3,638,790,000

5. How to combine 2 tables and have a complete 2016-2017 record?
```
SELECT * FROM S_P
UNION
SELECT * FROM SPIndex
GROUP BY date
```

6. What are the matching dates in 2 tables?
```
# a INNER JOIN clause will return a conjunction

SELECT t1.c1
FROM S_P as t1
INNER JOIN SPIndex as t2
ON t1.c1 = t2.c1
```
Results:

|   Date   |
|----------|
| 10/3/16  |
| 10/4/16  |
| 10/5/16  |
| 10/6/16  |
| ......   |
| 12/28/16 |
| 12/29/16 |
| 12/30/16 |

- October to December 2016 records exist in both databases

7. What are the Open and Close values from SPIndex, 2017 only?
```
# that is, to RIGHT JOIN SPIndex

SELECT t2.c1, t2.c2, t2.c5
FROM S_P as t1
RIGHT JOIN SPIndex as t2
ON t1.c1 = t2.c1
WHERE t1.c1 is NULL
```
Results:

| Date     | Open        | Close       |
|----------|-------------|-------------|
| 1/3/17   | 2251.570068 | 2257.830078 |
| 1/4/17   | 2261.600098 | 2270.75     |
| 1/5/17   | 2268.179932 | 2269        |
| 1/6/17   | 2271.139893 | 2276.97998  |
| ........ | ........... | ........... |
| 11/30/17 | 2633.929932 | 2647.580078 |
| 12/1/17  | 2645.100098 | 2642.219971 |
| 12/4/17  | 2657.189941 | 2639.439941 |
| 12/5/17  | 2639.780029 | 2629.570068 |

8. What is the total trading volume in 2017?

```
SELECT t2.c1, SUM(t2.c6)
FROM S_P as t1
RIGHT JOIN SPIndex as t2
ON t1.c1 = t2.c1
WHERE t1.c1 is NULL

= $802,741,450,000
```
