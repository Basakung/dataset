Python code for generate dataset:

import pandas as pd
import numpy as np
import random
from datetime import datetime, timedelta

# Generate random dates
def generate_dates(start_date, end_date):
    dates = pd.date_range(start=start_date, end=end_date, freq='D')
    return dates

# Generate synthetic dataset
def generate_retail_sales_data(num_records=1000):
    start_date = datetime(2023, 1, 1)
    end_date = datetime(2024, 1, 1)
    dates = generate_dates(start_date, end_date)
    
    categories = ['Electronics', 'Clothing', 'Groceries', 'Furniture', 'Toys', 'Beauty & Health', 'Sports & Outdoors', 'Automotive', 'Books & Stationery', 'Home Appliances']
    districts = ['Mueang Chiang Mai', 'Hang Dong', 'San Sai', 'Mae Rim', 'Saraphi', 'San Kamphaeng', 'Doi Saket']
    shopping_malls = ['Lotus', 'Makro', 'Jampha', 'Central Airport', 'Central Festival', 'BigC', 'Homepro']
    customer_ages = range(18, 80)
    customer_genders = ['Male', 'Female', 'Other']
    payment_methods = ['Credit Card', 'Debit Card', 'Cash', 'Digital Wallet']
    loyalty_levels = ['Bronze', 'Silver', 'Gold', 'Platinum']
    shopping_frequencies = ['Weekly', 'Bi-Weekly', 'Monthly']
    customer_salaries = range(10000, 100000, 5000)
    family_sizes = range(1, 8)
    
    data = []
    for date in dates:
        for district in districts:
            shopping_mall = random.choice(shopping_malls)
            
            # Restrict categories based on shopping mall
            if shopping_mall == 'Homepro':
                available_categories = ['Furniture', 'Home Appliances']
            elif shopping_mall in ['Jampha', 'Makro']:
                available_categories = [cat for cat in categories if cat != 'Clothing']
            else:
                available_categories = categories
                
            for category in available_categories:
                price = round(random.uniform(5, 500), 2)
                promotion = random.choice([0, 1])  # 0 = No promotion, 1 = Promotion applied
                sales_volume = max(1, int(np.random.normal(50, 20)))  # Normal distribution
                customer_age = random.choice(customer_ages)
                customer_gender = random.choice(customer_genders)
                payment_method = random.choice(payment_methods)
                loyalty_level = random.choice(loyalty_levels)
                shopping_frequency = random.choice(shopping_frequencies)
                customer_salary = random.choice(customer_salaries)
                family_size = random.choice(family_sizes)
                
                data.append([date, category, district, shopping_mall, price, promotion, sales_volume, customer_age, customer_gender, payment_method, loyalty_level, shopping_frequency, customer_salary, family_size])
    
    df = pd.DataFrame(data, columns=['Date', 'Category', 'District', 'Shopping_Mall', 'Price', 'Promotion', 'Sales_Volume', 'Customer_Age', 'Customer_Gender', 'Payment_Method', 'Loyalty_Level', 'Shopping_Frequency', 'Customer_Salary', 'Family_Size'])
    df.sort_values(by=['Date'], inplace=True)

    # Introduce missing data randomly
    missing_percentage = 0.1  # Set missing data to 10% of the data for each column
    for column in df.columns:
        num_missing = int(len(df) * missing_percentage)
        missing_indices = random.sample(range(len(df)), num_missing)
        df.loc[missing_indices, column] = np.nan

    return df


# Generate dataset with missing values
dataset = generate_retail_sales_data(5000)

# Save to CSV
dataset.to_csv('cm_retail_sales_data_with_missing.csv', index=False)

print("Dataset with missing data generated and saved as 'cm_retail_sales_data_with_missing.csv'")
