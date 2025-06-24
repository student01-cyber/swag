## OLAP operations

import pandas as pd

# Sample data
data = {
    'Region': ['North', 'North', 'South', 'South'],
    'Product': ['Laptop', 'Laptop', 'Phone', 'Phone'],
    'Year': [2023, 2023, 2023, 2023],
    'Quarter': ['Q1', 'Q2', 'Q1', 'Q2'],
    'Sales': [1000, 1200, 900, 1100]
}

df = pd.DataFrame(data)

# 1. Slice: Only for 2023
slice_df = df[df['Year'] == 2023]
print("SLICE:\n", slice_df)

# 2. Dice: 2023 + Product = 'Laptop'
dice_df = df[(df['Year'] == 2023) & (df['Product'] == 'Laptop')]
print("DICE:\n", dice_df)

# 3. Roll-up: Total sales by Year
rollup_df = df.groupby('Year')['Sales'].sum().reset_index()
print("ROLL-UP:\n", rollup_df)

# 4. Drill-down: Sales by Region and Quarter
drill_df = df.groupby(['Region', 'Quarter'])['Sales'].sum().reset_index()
print("DRILL-DOWN:\n", drill_df)

# 5. Pivot: Region as rows, Product as columns
pivot_df = df.pivot_table(values='Sales', index='Region', columns='Product', aggfunc='sum')
print("PIVOT:\n", pivot_df)





## mysql
import mysql.connector
import pandas as pd

# Step 1: Load CSV data using Pandas
df = pd.read_csv('students.csv')

# Step 2: Data Cleansing
df.dropna(inplace=True)  # Remove rows with missing values
df.drop_duplicates(inplace=True)  # Remove duplicate rows

# Step 3: Connect to MySQL
conn = mysql.connector.connect(
    host="localhost",
    user="root",
    password="Svkm@2004",
    database="college"
)
cursor = conn.cursor()

# Step 4: Insert data into MySQL table
for index, row in df.iterrows():
    cursor.execute("INSERT INTO students (name, age, marks) VALUES (%s, %s, %s)", 
                   (row['name'], row['age'], row['marks']))
conn.commit()

# Step 5: Retrieve and display data
cursor.execute("SELECT * FROM students WHERE marks > 75")
result = cursor.fetchall()
for row in result:
    print(row)

conn.close()



## star

Create Table Branch (Branch_id INT, Branch_name varchar(25))

Create Table Employee (Employee_id INT, Employee_Name varchar(25))

QL Server 16.0.100

Create Table Product (Product_id INT, Product_name varchar(25))

Create Table Customer (Customer_id INT, Customer_name varchar(25))

ots

Create Table Fact (Branch_id INT, Employee_id INT, Product_id INT, Customer_id INT)
