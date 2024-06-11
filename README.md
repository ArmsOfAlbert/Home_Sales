# Home_Sales
# Home Sales Analysis

This project analyzes home sales data to derive insights such as average prices based on various criteria, including the number of bedrooms, bathrooms, and the year built. The data is processed and queried using Apache Spark.

## Prerequisites

- Apache Spark 3.4.0 or later
- Python 3.10.13 or later
- Java 8/11/17
- Necessary Python libraries (pyspark, pandas, etc.)


## Data Preparation

1. **Place your dataset:**
    - Ensure your home sales dataset (e.g., `home_sales.csv`) is placed in the `data/` directory.

2. **Convert CSV to Parquet (if needed):**
    ```python
    from pyspark.sql import SparkSession

    spark = SparkSession.builder.appName("HomeSales").getOrCreate()
    df = spark.read.csv("data/home_sales.csv", header=True, inferSchema=True)
    df.write.parquet("data/home_sales.parquet")
    ```



## Queries Performed

1. **Average Price for Four Bedroom Houses per Year:**
    ```python
    avg_price = df.filter(df.bedrooms == 4).groupBy("year").agg({"price": "avg"}).orderBy("year")
    avg_price.show()
    ```

2. **Average Price for Homes Built Each Year with 3 Bedrooms and 3 Bathrooms:**
    ```python
    avg_price_3b3b = df.filter((df.bedrooms == 3) & (df.bathrooms == 3)).groupBy("year").agg({"price": "avg"}).orderBy("year")
    avg_price_3b3b.show()
    ```

3. **Average Price for Homes with Specific Criteria:**
    ```python
    avg_price_specific = df.filter((df.bedrooms == 3) & (df.bathrooms == 3) & (df.floors == 2) & (df.sqft >= 2000)).groupBy("year").agg({"price": "avg"}).orderBy("year")
    avg_price_specific.show()
    ```

4. **Average Price per "View" Rating:**
    ```python
    avg_price_view = df.groupBy("view").agg({"price": "avg"}).filter("avg(price) >= 350000").orderBy("view", ascending=False)
    avg_price_view.show()
    ```

## Caching and Performance

1. **Cache Table:**
    ```python
    df.createOrReplaceTempView("home_sales")
    spark.sql("CACHE TABLE home_sales")
    ```

2. **Check if Table is Cached:**
    ```python
    is_cached = spark.catalog.isCached("home_sales")
    print("Is home_sales cached?", is_cached)
    ```

3. **Uncache Table:**
    ```python
    spark.sql("UNCACHE TABLE home_sales")
    ```

