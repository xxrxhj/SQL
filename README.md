# S&P 500 Index Fund OHLCV (2016-2017)

## Known for its decent risk profile, S&P 500 is an index of top performing stocks. The following 2 databases included the open-high-low-close-volume data from 2016 through 2017. Let's take a look at the tables and find some key insights there.

1. select the databases
```
SELECT * FROM S_P  
# this gives us 2016 records
```

```
SELECT * FROM SPIndex
# return 2016-2017 partial data
```

2. How many days are there and what is the average open?
```
SELECT COUNT(DISTINCT Date), AVG(Open)
FROM S_P
# there are 253 rows and an opening of 2086 on average
```
3. What are the dates that have higher open than close values?
```
SELECT Date
FROM S_P
GROUP BY Date
HAVING AVG(Open) > (SELECT AVG(Close) 
                    from S_P)
Order by Date DESC

# 15 days in total, e.g. 
9/30/16
9/29/16
9/26/16
9/23/16
9/22/16
9/21/16
8/9/16
8/8/16
8/5/16
8/4/16
```
4. What are the dates that have the highest volume in SPIndex?

```
# from 1 we've seen that SPIndex had 2017 data
# volume indicates total number of shares traded. Larger value means the stock is appealing to buyers
# let's find out when the stocks are most desirable

SELECT volume, max(date) as latest_date
FROM SPIndex
GROUP BY volume
ORDER BY latest_date ASC

# turns out that 1/10/17 has the largest volume at 3638790000

5. 
