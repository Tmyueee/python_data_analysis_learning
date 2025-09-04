Python Learning Journal
Under the guidance of GPT, I am revisiting Python basics and data analysis, focusing on practicing data manipulation with Pandas.
Data source: https://www.kaggle.com/datasets/adilshamim8/tesla-stock-price-history

Contents
- Data Loading & Inspection
- loc & iloc
- Boolean Indexing
- Missing Values Handling
- Data Type Conversion


1️⃣ Data Loading & Inspection
import pandas as pd

# Read a CSV file and return a DataFrame
df = pd.read_csv('Tesla_stock_data.csv')

# Check the shape of the DataFrame (rows, columns)
print(df.shape) 

# Show the first 5 rows by default
print(df.head()) 

# View the last 5 rows
df.tail()  

# We can change the parameter
# View the first 10 rows
df.head(10) 

# View basic information of the DataFrame: columns, data types, non-null counts
print(df.info()) 

# Check the number of missing values in each column
print(df.isna().sum()) 

# Show descriptive statistics of numeric columns: mean, std, min, max, etc.
print(df.describe()) 


2️⃣ loc & iloc
- loc
select rows/columns by their labels
supports row/column names, boolean conditions, date ranges
slice is inclusive (both ends included)

-iloc
select rows/columns by integer position
rows/columns start from 0


eg.
    A   B
x  10  50
y  20  60
z  30  70
w  40  80

df.loc["x"]          # row with index label 'x'
df.loc["y":"z"]      # rows from 'y' to 'z' (inclusive)
df.loc[:, "A"]       # column 'A'
df.loc["x", "B"]     # value at row 'x' and column 'B' -> 50
df.loc[df["A"] > 20] # all rows where A > 20

df.iloc[0]         # first row (row at position 0 -> 'x')
df.iloc[1:3]       # rows 1 to 2 (exclusive of position 3)
df.iloc[:, 0]      # first column (A)
df.iloc[0, 1]      # element at row 0, col 1 -> 50


Exercises
-----Basic Exercises-----
# 1. Use .loc to select all data of the first row
df.loc[0]

# 2. Use .loc to select Date and Close columns for the first 5 rows
df.loc[0:4, ['Date','Close']]

# 3. Use .iloc to get the Close value of the first row
df['Close'].iloc[0]

# 4. Use .iloc to get rows 2–6 (excluding row 6), first 3 columns
df.iloc[1:5, 0:3]


-----Advanced Exercises-----
# 5. Find Date and Close where Close price > 450
df.loc[df['Close'] > 450, ['Date','Close']]

# 6. Find records where Volume < median, show Date, Close, Volume
df.loc[df['Volume'] < df['Volume'].median(), ['Date','Close','Volume']]

# 7. Find records after 2020-08-01, show Date, Close
df['Date'] = pd.to_datetime(df['Date'])   # Convert Date column to datetime first for safety
df.loc[df['Date'] > '2020-08-01', ['Date', 'Close']]

# 8. Find records where Close > mean and Volume > median
df.loc[(df['Close'] > df['Close'].mean()) & (df['Volume'] > df['Volume'].median()), ['Date','Close','Volume']]  # () & ()

# 9. Find records where Close < 500 or Volume > mean
df.loc[(df['Close'] < 500) | (df['Volume'] > df['Volume'].mean()), ['Date','Close','Volume']]

# 10. Find records with Close > 450, then select columns 0,1,4
df.loc[df['Close'] > 450].iloc[:, [0,1,4]]


-----Challenge Tasks-----
# 11. Count number of days where Close > Open
df.loc[df['Close'] > df['Open'], 'Date'].count()

# 12. Get last 10 rows (Date, Close) and sort by Close descending
df.iloc[-10:, [1,2]].sort_values(by='Close')

# 13. Find records where Close > mean and Volume > mean
df.loc[(df['Close'] > df['Close'].mean()) & (df['Volume'] > df['Volume'].mean()), ['Date','Close','Volume']]


3️⃣ Boolean Indexing
本质上就是条件筛选的延伸
It is essentially an extension of conditional filtering.

区别是直接用布尔 Series 来选数据，而不是写在 loc 里
The difference is that you use a boolean Series directly to select data, instead of writing it inside .loc.

eg. 
mask = df['Close'] > 1000
df[mask]

用途：可以组合多个条件、复用筛选逻辑
Usage: It allows combining multiple conditions and reusing filtering logic.


Exercises
# 1. Filter rows where Close > 450
con = df['Close'] > 450
df[con]

# 2. Find records where Volume < mean
con1 = df['Volume'] < df['Volume'].mean()
df[con1]

# 3. Find records where Close is between 300 and 400
con2 = (df['Close'] >= 300) & (df['Close'] <= 400)
df[con2]

# 4. Find records where Close > mean and Volume < median
con3 = (df['Close'] > df['Close'].mean()) & (df['Volume'] < df['Volume'].median())
df[con3]


4️⃣ Missing Values Handling
Check missing values: 检查缺失值：
df.isnull().sum()

Drop missing values:删除缺失值：
df.dropna()

Fill missing values:填充缺失值:
df.fillna(0)                 # Fill with 0 用 0 填充


Exercises
# Fill missing values in Close with mean
df['Close'].fillna(df['Close'].mean())


5️⃣ Data Type Conversion
# 查看数据类型 Check data types
df.dtypes

# 转换：Convert:
df['Date'] = pd.to_datetime(df['Date'])  # 转日期
df['Volume'] = df['Volume'].astype(float)

#常见数据类型： Common data types:
int, float, str, bool, category


Exercise
# 1. Convert Volume to float
df['Volume'].astype(float)

# 2. Convert Date to string
df['Date'].astype(str)

# 3. Convert Close to boolean: True if Close > 400
df['Close_bool'] = (df['Close'] > 400).astype(bool)

# 4. Convert Date to datetime and extract year into a new column 'Year'
df['Date'] = pd.to_datetime(df['Date'])  
df['Year'] = df['Date'].dt.year         



Practice makes perfect!











