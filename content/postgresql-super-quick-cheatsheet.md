<p align="center">
    <img src="./../assets/psql-super-quick-cheatsheet.png" alt="Super Quick PostgreSQL Cheatsheet by Danny Ma">
</p>

[![forthebadge](./../assets/badges/version-1.0.svg)]()
[![forthebadge](https://forthebadge.com/images/badges/powered-by-coffee.svg)]()
[![forthebadge](https://forthebadge.com/images/badges/built-with-love.svg)]()
[![forthebadge](https://forthebadge.com/images/badges/ctrl-c-ctrl-v.svg)]()

**PostgreSQL Super Quick Cheatsheet**

- [Basic Operations](#basic-operations)
  - [Return Top 5 Rows](#return-top-5-rows)
  - [Sort Outputs in Alphabetical Order](#sort-outputs-in-alphabetical-order)
  - [Where Filter on Strong Column](#where-filter-on-strong-column)
  - [Where Filter for Not Equal](#where-filter-for-not-equal)
  - [Multiple Where Filters with Dates](#multiple-where-filters-with-dates)
  - [Get Unique Vales From Column](#get-unique-vales-from-column)
  - [Group By Aggregates](#group-by-aggregates)
  - [Cast Float Datatypes for Rounding](#cast-float-datatypes-for-rounding)
- [Date Manipulation](#date-manipulation)
  - [Extract Year From Date](#extract-year-from-date)
  - [Extract 1st Day of Month](#extract-1st-day-of-month)
- [Basic String Operations](#basic-string-operations)
  - [Get 1st Character From String Column](#get-1st-character-from-string-column)
  - [Get Last Character From String Column](#get-last-character-from-string-column)
  - [Remove Final Character from String Column](#remove-final-character-from-string-column)
- [Case When Statements](#case-when-statements)
  - [Case When Statements for Column Transformations](#case-when-statements-for-column-transformations)
  - [Sum Case When for Excel COUNTIF](#sum-case-when-for-excel-countif)
  - [Sum Case When for Excel SUMIF](#sum-case-when-for-excel-sumif)
- [Window Functions](#window-functions)
  - [Ranking Function](#ranking-function)
  - [7 day moving average](#7-day-moving-average)
  - [Cumulative Sum](#cumulative-sum)
  - [Lag Function](#lag-function)
- [Table Joins](#table-joins)
  - [Inner Join](#inner-join)
  - [Left Join](#left-join)
  - [Multiple Table Joins](#multiple-table-joins)

# Basic Operations

```sql
-- Show all rows and columns
SELECT * FROM trading.members;
```

| member_id | first_name |    region     |
| --------- | ---------- | ------------- |
| c4ca42    | Danny      | Australia     |
| c81e72    | Vipul      | United States |
| eccbc8    | Charlie    | United States |
| a87ff6    | Nandita    | United States |
| e4da3b    | Rowan      | United States |
| 167909    | Ayush      | United States |
| 8f14e4    | Alex       | United States |
| c9f0f8    | Abe        | United States |
| 45c48c    | Ben        | Australia     |
| d3d944    | Enoch      | Africa        |
| 6512bd    | Vikram     | India         |
| c20ad4    | Leah       | Asia          |
| c51ce4    | Pavan      | Australia     |
| aab323    | Sonia      | Australia     |

## Return Top 5 Rows

```sql
-- Return top 5 rows
SELECT * FROM trading.members
LIMIT 5;
```

| member_id | first_name |    region     |
| --------- | ---------- | ------------- |
| c4ca42    | Danny      | Australia     |
| c81e72    | Vipul      | United States |
| eccbc8    | Charlie    | United States |
| a87ff6    | Nandita    | United States |
| e4da3b    | Rowan      | United States |

## Sort Outputs in Alphabetical Order

```sql
-- Order in alphabetical order
SELECT * FROM trading.members
ORDER BY first_name;
```

| member_id | first_name |    region     |
| --------- | ---------- | ------------- |
| c9f0f8    | Abe        | United States |
| 8f14e4    | Alex       | United States |
| 167909    | Ayush      | United States |
| 45c48c    | Ben        | Australia     |
| eccbc8    | Charlie    | United States |
| c4ca42    | Danny      | Australia     |
| d3d944    | Enoch      | Africa        |
| c20ad4    | Leah       | Asia          |
| a87ff6    | Nandita    | United States |
| c51ce4    | Pavan      | Australia     |
| e4da3b    | Rowan      | United States |
| aab323    | Sonia      | Australia     |
| 6512bd    | Vikram     | India         |
| c81e72    | Vipul      | United States |

```sql
-- Record counts
SELECT COUNT(*) FROM trading.prices;
```

| count | 
| ----- |
|  3404 |

## Where Filter on Strong Column

```sql
-- Where Filter on String Column
SELECT * FROM trading.members
WHERE region = 'United States';
```

| member_id | first_name |    region     |
| --------- | ---------- | ------------- |
| c81e72    | Vipul      | United States |
| eccbc8    | Charlie    | United States |
| a87ff6    | Nandita    | United States |
| e4da3b    | Rowan      | United States |
| 167909    | Ayush      | United States |
| 8f14e4    | Alex       | United States |
| c9f0f8    | Abe        | United States |

## Where Filter for Not Equal

```sql
-- Where filter for not equals
SELECT * FROM trading.members
WHERE region != 'Australia';
```

| member_id | first_name |    region     |
| --------- | ---------- | ------------- |
| c81e72    | Vipul      | United States |
| eccbc8    | Charlie    | United States |
| a87ff6    | Nandita    | United States |
| e4da3b    | Rowan      | United States |
| 167909    | Ayush      | United States |
| 8f14e4    | Alex       | United States |
| c9f0f8    | Abe        | United States |
| d3d944    | Enoch      | Africa        |
| 6512bd    | Vikram     | India         |
| c20ad4    | Leah       | Asia          |

## Multiple Where Filters with Dates

```sql
SELECT
  COUNT(*) AS transaction_count
FROM trading.transactions
WHERE txn_type = 'BUY'
  AND ticker = 'BTC'
  AND txn_date BETWEEN '2020-01-01' AND '2020-12-31';
```

| transaction_count |
| ----------------- |
|              2350 |

## Get Unique Vales From Column

```sql
-- Get unique values from column
SELECT DISTINCT region FROM trading.members;
```

|    region     |
| ------------- |
| United States |
| Australia     |
| India         |
| Africa        |
| Asia          |

## Group By Aggregates

```sql
-- Group by aggregates
SELECT
  ticker,
  COUNT(*) AS record_count,
  MIN(price) AS min_price,
  MAX(price) AS max_price
FROM trading.prices
GROUP BY ticker;
```

| ticker | record_count | min_price | max_price |
| ------ | ------------ | --------- | --------- |
| BTC    |         1702 |     785.4 |   63540.9 |
| ETH    |         1702 |      8.20 |   4167.78 |

## Cast Float Datatypes for Rounding

```sql
-- Cast float datatypes for rounding
SELECT
  ROUND(AVG(price)::NUMERIC, 2)
FROM trading.prices;
```

|  round  |
| ------- |
| 6645.43 |

# Date Manipulation

## Extract Year From Date

```sql
-- Extract year from date
SELECT DISTINCT
  EXTRACT(YEAR FROM market_date) AS calendar_year
FROM trading.prices
ORDER BY calendar_year;
```

| calendar_year |
| ------------- |
|          2017 |
|          2018 |
|          2019 |
|          2020 |
|          2021 |

## Extract 1st Day of Month

```sql
-- Return 1st day of month as YYYY-MM-DD
SELECT
  DATE_TRUNC('MON', txn_date)::DATE AS month_start,
  COUNT(*) AS transaction_count
FROM trading.transactions
WHERE txn_type = 'BUY'
  AND ticker = 'BTC'
  AND txn_date BETWEEN '2020-01-01' AND '2020-12-31'
GROUP BY month_start
ORDER BY month_start;
```

| month_start | transaction_count |
| ----------- | ----------------- |
| 2020-01-01  |               215 |
| 2020-02-01  |               175 |
| 2020-03-01  |               180 |
| 2020-04-01  |               199 |
| 2020-05-01  |               196 |
| 2020-06-01  |               174 |
| 2020-07-01  |               204 |
| 2020-08-01  |               195 |
| 2020-09-01  |               207 |
| 2020-10-01  |               207 |
| 2020-11-01  |               203 |
| 2020-12-01  |               195 |

# Basic String Operations

## Get 1st Character From String Column

```sql
-- Get 1st character from string column
SELECT
  first_name,
  LEFT(first_name, 1) AS first_character
FROM trading.members;
```

| first_name | first_character |
| ---------- | --------------- |
| Danny      | D               |
| Vipul      | V               |
| Charlie    | C               |
| Nandita    | N               |
| Rowan      | R               |
| Ayush      | A               |
| Alex       | A               |
| Abe        | A               |
| Ben        | B               |
| Enoch      | E               |
| Vikram     | V               |
| Leah       | L               |
| Pavan      | P               |
| Sonia      | S               |

## Get Last Character From String Column

```sql
-- Get last character from string column
SELECT
  first_name,
  RIGHT(first_name, 1) AS last_character
FROM trading.members;
```

| first_name | last_character |
| ---------- | -------------- |
| Danny      | y              |
| Vipul      | l              |
| Charlie    | e              |
| Nandita    | a              |
| Rowan      | n              |
| Ayush      | h              |
| Alex       | x              |
| Abe        | e              |
| Ben        | n              |
| Enoch      | h              |
| Vikram     | m              |
| Leah       | h              |
| Pavan      | n              |
| Sonia      | a              |

## Remove Final Character from String Column

```sql
-- Remove final character from string column
SELECT
  volume,
  LEFT(volume, LENGTH(volume)-1) AS adjusted_volume, 1
FROM trading.prices
LIMIT 10;
```

| volume  | adjusted_volume |
| ------- | --------------- |
| 582.04K | 582.04          |
| 466.21K | 466.21          |
| 839.54K | 839.54          |
| 118.44K | 118.44          |
| 923.13K | 923.13          |
| 988.82K | 988.82          |
| 1.09M   | 1.09            |
| 747.65K | 747.65          |
| 768.74K | 768.74          |
| 739.32K | 739.32          |

# Case When Statements

## Case When Statements for Column Transformations

```sql
-- Use case when to apply logical transformations
SELECT
  volume,
  CASE
    WHEN RIGHT(volume, 1) = 'K'
      THEN LEFT(volume, LENGTH(volume)-1)::NUMERIC * 1000
    WHEN RIGHT(volume, 1) = 'M'
      THEN LEFT(volume, LENGTH(volume)-1)::NUMERIC * 1000000
    WHEN volume = '-' THEN 0
  END AS adjusted_volume
FROM trading.prices
LIMIT 10;
```

| volume  | adjusted_volume |
| ------- | --------------- |
| 582.04K |       582040.00 |
| 466.21K |       466210.00 |
| 839.54K |       839540.00 |
| 118.44K |       118440.00 |
| 923.13K |       923130.00 |
| 988.82K |       988820.00 |
| 1.09M   |      1090000.00 |
| 747.65K |       747650.00 |
| 768.74K |       768740.00 |
| 739.32K |       739320.00 |

## Sum Case When for Excel COUNTIF

```sql
-- Sum case when for Excel style COUNTIF function
SELECT
  ticker,
  SUM(CASE WHEN price > open THEN 1 ELSE 0 END) AS breakout_days,
  SUM(CASE WHEN price <= open THEN 1 ELSE 0 END) AS non_breakout_days
FROM trading.prices
GROUP BY ticker;
```

| ticker | breakout_days | non_breakout_days |
| ------ | ------------- | ----------------- |
| BTC    |           921 |               781 |
| ETH    |           887 |               815 |

## Sum Case When for Excel SUMIF

```sql
-- Sum Case When for Excel Style SUMIF Function
SELECT
  ticker,
  SUM(
    CASE
      WHEN txn_type = 'SELL' THEN -quantity
      ELSE quantity
    END
  ) AS final_btc_holding
FROM trading.transactions
GROUP BY ticker;
```

| ticker |    final_btc_holding     |
| ------ | ------------------------ |
| BTC    | 42848.666853848498952415 |
| ETH    |  32801.04227030932968988 |

# Window Functions

## Ranking Function

```sql
-- CTE Common Table Expressions with Rank
WITH cte_rank AS (
SELECT
  ticker,
  market_date,
  price,
  volume,
  RANK() OVER (
    PARTITION BY ticker
    ORDER BY price DESC
  ) AS price_rank
FROM trading.prices
)
SELECT * FROM cte_rank
WHERE price_rank <= 5
ORDER BY ticker, price_rank;
```

| ticker | market_date |  price  | volume  | price_rank |
| ------ | ----------- | ------- | ------- | ---------- |
| BTC    | 2021-04-13  | 63540.9 | 126.56K |          1 |
| BTC    | 2021-04-15  | 63216.0 | 76.97K  |          2 |
| BTC    | 2021-04-14  | 62980.4 | 130.43K |          3 |
| BTC    | 2021-04-16  | 61379.7 | 136.85K |          4 |
| BTC    | 2021-03-13  | 61195.3 | 134.64K |          5 |
| ETH    | 2021-05-11  | 4167.78 | 1.27M   |          1 |
| ETH    | 2021-05-14  | 4075.38 | 2.06M   |          2 |
| ETH    | 2021-05-10  | 3947.90 | 2.70M   |          3 |
| ETH    | 2021-05-09  | 3922.23 | 1.94M   |          4 |
| ETH    | 2021-05-08  | 3905.55 | 1.34M   |          5 |


## 7 day moving average

```sql
-- 7 day moving average window function
SELECT
  ticker,
  market_date,
  price,
  AVG(price) OVER (
    PARTITION BY ticker
    ORDER BY market_date
    RANGE BETWEEN '7 DAYS' PRECEDING AND CURRENT ROW
  ) AS moving_avg_price
FROM trading.prices
LIMIT 10;
```

| ticker | market_date | price  |   moving_avg_price    |
| ------ | ----------- | ------ | --------------------- |
| BTC    | 2017-01-01  |  995.4 |  995.4000000000000000 |
| BTC    | 2017-01-02  | 1017.0 | 1006.2000000000000000 |
| BTC    | 2017-01-03  | 1033.3 | 1015.2333333333333333 |
| BTC    | 2017-01-04  | 1135.4 | 1045.2750000000000000 |
| BTC    | 2017-01-05  |  989.3 | 1034.0800000000000000 |
| BTC    | 2017-01-06  |  886.2 | 1009.4333333333333333 |
| BTC    | 2017-01-07  |  888.9 |  992.2142857142857143 |
| BTC    | 2017-01-08  |  900.9 |  980.8000000000000000 |
| BTC    | 2017-01-09  |  899.8 |  968.8500000000000000 |
| BTC    | 2017-01-10  |  904.4 |  954.7750000000000000 |

## Cumulative Sum

```sql
-- Cumulative Sum with `DISTINCT` and `RANGE` Frame Mode
SELECT DISTINCT
  ticker,
  DATE_TRUNC('MON', txn_date)::DATE AS month_start,
  -- try changing RANGE to ROWS to see what happens...
  SUM(quantity) OVER (
    PARTITION BY ticker
    ORDER BY DATE_TRUNC('MON', txn_date)
    RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
  ) AS cumulative_monthly_volume
FROM trading.transactions
WHERE txn_type = 'BUY'
ORDER BY ticker, month_start
LIMIT 10;
```

| ticker | month_start | cumulative_monthly_volume |
| ------ | ----------- | ------------------------- |
| BTC    | 2017-01-01  |     1722.2024245893620006 |
| BTC    | 2017-02-01  |    2474.16941832516762386 |
| BTC    | 2017-03-01  |    3303.60330168213998525 |
| BTC    | 2017-04-01  |    4156.74518942893330155 |
| BTC    | 2017-05-01  |    4926.57357466275834135 |
| BTC    | 2017-06-01  |    5889.91822587306524865 |
| BTC    | 2017-07-01  |    7007.98391765702142765 |
| BTC    | 2017-08-01  |    8005.32251139291769365 |
| BTC    | 2017-09-01  |    9005.18505015029428155 |
| BTC    | 2017-10-01  |   10087.51552082696338105 |


## Lag Function

```sql
-- Lag Window Function
SELECT
  ticker,
  market_date,
  price,
  LAG(price) OVER (
    PARTITION BY ticker
    ORDER BY market_date
  ) AS previous_price
FROM trading.prices
ORDER BY ticker, market_date
LIMIT 10;
```

| ticker | market_date | price  | previous_price |
| ------ | ----------- | ------ | -------------- |
| BTC    | 2017-01-01  |  995.4 |                |
| BTC    | 2017-01-02  | 1017.0 |          995.4 |
| BTC    | 2017-01-03  | 1033.3 |         1017.0 |
| BTC    | 2017-01-04  | 1135.4 |         1033.3 |
| BTC    | 2017-01-05  |  989.3 |         1135.4 |
| BTC    | 2017-01-06  |  886.2 |          989.3 |
| BTC    | 2017-01-07  |  888.9 |          886.2 |
| BTC    | 2017-01-08  |  900.9 |          888.9 |
| BTC    | 2017-01-09  |  899.8 |          900.9 |
| BTC    | 2017-01-10  |  904.4 |          899.8 |

# Table Joins

## Inner Join

```sql
-- Inner Join Between 2 Tables
SELECT
  members.region,
  SUM(transactions.quantity) AS bitcoin_quantity
FROM trading.transactions
INNER JOIN trading.members
  ON transactions.member_id = members.member_id
WHERE transactions.ticker = 'BTC'
  AND transactions.txn_type = 'BUY'
GROUP BY members.region
ORDER BY bitcoin_quantity DESC;
```

|    region     |     bitcoin_quantity     |
| ------------- | ------------------------ |
| United States | 25704.872086133242842065 |
| Australia     |   14266.9792516449027162 |
| Asia          |   4975.75064119164404784 |
| Africa        |    4270.8573823425313401 |
| India         |    4031.6925788360780822 |

## Left Join

```sql
-- Left Join Between 2 Tables
SELECT
  prices.market_date,
  COUNT(transactions.txn_id) AS transaction_count
FROM trading.prices
LEFT JOIN trading.transactions
  ON prices.market_date = transactions.txn_date
  AND prices.ticker = transactions.ticker
GROUP BY prices.market_date
HAVING COUNT(transactions.txn_id) < 5
ORDER BY prices.market_date DESC;
```

| market_date | transaction_count |
| ----------- | ----------------- |
| 2021-08-29  |                 0 |
| 2021-08-28  |                 0 |
| 2021-07-17  |                 3 |
| 2021-01-06  |                 4 |
| 2020-01-17  |                 4 |
| 2019-07-15  |                 4 |
| 2019-06-14  |                 3 |
| 2018-10-20  |                 4 |


## Multiple Table Joins

```sql
-- Multiple Table Joins
SELECT
  members.region,
  SUM(transactions.quantity) AS btc_quantity,
  AVG(prices.price) AS avg_btc_price
FROM trading.transactions
INNER JOIN trading.prices
  ON transactions.ticker = prices.ticker
  AND transactions.txn_date = prices.market_date
INNER JOIN trading.members
  ON transactions.member_id = members.member_id
WHERE transactions.ticker = 'BTC'
  AND transactions.txn_type = 'BUY'
GROUP BY members.region
ORDER BY avg_btc_price DESC;
```

|    region     |       btc_quantity       |   avg_btc_price    |
| ------------- | ------------------------ | ------------------ |
| Asia          |   4975.75064119164404784 | 13138.019289340102 |
| Africa        |    4270.8573823425313401 | 12804.277195121951 |
| United States | 25704.872086133242842065 | 12640.233690051526 |
| Australia     |   14266.9792516449027162 | 12521.179835538077 |
| India         |    4031.6925788360780822 | 12311.020202020202 |