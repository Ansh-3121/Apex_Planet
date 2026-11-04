# Apex Planet Retail Sales Analysis

I did this project as part of my ApexPlanet data analytics internship. The dataset is a retail sales file with 1,000 orders, 
and I wanted to find out who is buying what, which cities bring in the most money, and whether anything odd shows up in the numbers.

I cleaned the data in Excel, answered business questions in MySQL, and then did the exploration and charts in
Python (Pandas, NumPy, Matplotlib) using Jupyter Notebook.

## The dataset

1,000 rows and 12 columns:

| Column | What it is |
|---|---|
| Order_ID | Unique order number |
| Order_Date | Date of the order |
| Customer_ID | Customer identifier |
| Customer_Name | Customer name |
| Age | Customer age |
| Gender | Male / Female |
| City | Customer's city |
| Product | Product bought |
| Category | Product category |
| Quantity | Units bought |
| Unit_Price | Price per unit |
| Total_Sales | Quantity x Unit_Price |

## 1. Cleaning in Excel

Not much was broken, but two columns needed attention:

- **City** had some blank cells. I filled them with `Unknown` instead of deleting the rows, because the sales in those rows still count.
- **Age** had a few missing values. I first looked for outliers (there were none, ages stay between 18 and 65), and then filled the gaps with the average age.

## 2. SQL analysis (MySQL)

### Sales by gender

| Gender | Total Sales |
|---|---:|
| Female | 66,935,889 |
| Male | 72,463,551 |

Men spent more in total, even though the number of orders is almost equal (511 vs 489).

### Top category in each city

| City | Top Category | Category Sales |
|---|---|---:|
| Bengaluru | Electronics | 492,174.10 |
| Delhi | Electronics | 444,016.40 |
| Gaya | Electronics | 439,499.43 |
| Hyderabad | Grocery | 482,551.90 |
| Kolkata | Electronics | 485,667.80 |
| Mumbai | Grocery | 405,004.41 |
| Patna | Education | 493,677.50 |
| Pune | Fashion | 462,701.30 |
| Unknown | Education | 439,469.60 |

Electronics wins in four cities. The rest are split between Grocery, Education and Fashion.

### Top 3 products by sales

| Rank | Product | Orders | Total Sales |
|---:|---|---:|---:|
| 1 | Laptop | 170 | 25,443,009 |
| 2 | Mobile | 184 | 25,335,573 |
| 3 | Book | 178 | 25,031,689 |

Interesting that Laptop is first even though Mobile and Book have more orders. The three are very close though.

### Top 10 customers

| Rank | Customer | Total Purchases |
|---:|---|---:|
| 1 | Customer_335 | 1,684,833 |
| 2 | Customer_138 | 1,305,933 |
| 3 | Customer_266 | 1,269,445 |
| 4 | Customer_375 | 1,196,934 |
| 5 | Customer_274 | 1,060,340 |
| 6 | Customer_343 | 1,025,447 |
| 7 | Customer_415 | 967,962 |
| 8 | Customer_355 | 954,514 |
| 9 | Customer_1 | 952,754 |
| 10 | Customer_359 | 941,957 |

The SQL side uses GROUP BY, CTEs and window functions (ranking) for the "top per city" and "top N" questions.

## 3. Python analysis

### Basic checks

I loaded the CSV with Pandas, checked `df.info()` for data types and nulls, converted `Order_Date` to datetime, and used `describe()` and
`value_counts()` to get a feel for the data.

- Total sales: **139,399,439.65**
- Records: **1,000**
- Average age: **41.35** (min 18, max 65)

### Distributions

| Gender | Orders |
|---|---:|
| Male | 511 |
| Female | 489 |

| Product | Orders |
|---|---:|
| Mobile | 184 |
| Book | 178 |
| Laptop | 170 |
| Chair | 159 |
| Shoes | 156 |
| Rice | 153 |

| Category | Orders |
|---|---:|
| Electronics | 354 |
| Education | 178 |
| Furniture | 159 |
| Fashion | 156 |
| Grocery | 153 |

Electronics has the most orders simply because it holds two products (Mobile and Laptop).

### Sales by category

| Category | Total Sales |
|---|---:|
| Electronics | 50,778,582 |
| Education | 25,031,689 |
| Grocery | 22,231,711 |
| Furniture | 21,521,561 |
| Fashion | 19,835,896 |

Electronics brings in roughly double what any other category does.

### Category-wise sales by gender

![Category-wise Sales by Gender]("C:\Users\HP\Pictures\Screenshots\Screenshot 2026-11-04 141933.png")

I made this stacked bar chart to see whether the male lead in total sales is spread across everything or comes from one place.
Electronics is clearly the biggest bar, and since it is so large, most of the gap between male and female sales comes from there.
In the other four categories the two genders are fairly close to each other, and in a couple of them the female share looks equal or slightly higher.
So I would not say "men buy more of everything", it is mostly an Electronics story.

### Sales by city

| City | Total Sales |
|---|---:|
| Patna | 19,285,967 |
| Kolkata | 18,884,350 |
| Bengaluru | 18,773,574 |
| Mumbai | 18,757,050 |
| Hyderabad | 17,166,767 |
| Delhi | 16,097,079 |
| Pune | 14,513,176 |
| Gaya | 14,380,859 |
| Unknown | 1,540,618 |

Patna is first, but the top four cities are only a few hundred thousand apart.

### Monthly trend

I plotted monthly sales as a line chart. March and June were the strongest months, sales dropped in August and September, and they picked up again from October.

### Age distribution

A histogram of customer age. Customers are spread across 18 to 65 with an average of about 41, so there is no single age group that dominates.

### Total sales distribution (outlier check)

![Total Sales Boxplot]("C:\Users\HP\Pictures\Screenshots\Screenshot 2026-11-04 142123.png")

I used a boxplot on `Total_Sales` to check how spread out the order values are and whether there are outliers. What I noticed:

- The middle 50% of orders sit roughly between 60,000 and 210,000, and the median is somewhere around 120,000.
- The mean (the small triangle) is higher than the median, so the data is right-skewed. A few big orders pull the average up.
- There is a small group of outliers above about 440,000, going up to nearly 500,000.

I did not remove these outliers. They look like genuine high-value orders (a high quantity of an expensive product like a laptop) and not data errors, and removing them would change the total sales figure.

## 4. Key findings

- Total sales came to about 139.4 million.
- Men generated more sales than women, mostly because of Electronics.
- Electronics is the top category by a big margin.
- Laptop is the top product by sales.
- Patna is the top city, but the next few cities are very close.
- Sales dipped in August and September.
- Order values are right-skewed, with a few very large orders above 440,000.
- Customer ages range from 18 to 65.

## 5. Skills used

- Data cleaning and handling missing values
- Exploratory data analysis
- SQL: GROUP BY, CTEs, window functions
- Pandas and NumPy
- Charts with Matplotlib (line, histogram, stacked bar, boxplot)
- Outlier detection
- Turning numbers into business insights

## 6. Folder structure

```text
Apex-Planet-Retail-Sales-Analysis/
│
├── data/
│   └── Apex_Planet.csv
│
├── notebooks/
│   └── Retail_Sales_Analysis.ipynb
│
├── screenshots/
│   ├── category_wise_sales.png
│   ├── category_sales_by_gender.png
│   ├── city_wise_sales.png
│   ├── monthly_sales_trend.png
│   ├── customer_age_distribution.png
│   └── total_sales_boxplot.png
│
└── README.md
```
