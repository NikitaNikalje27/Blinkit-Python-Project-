#Blinkit Python Project 
#Data Analysis 

# import library
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
## import data
df = pd.read_csv("C:/Users/NIKITA/blinkit_data.csv")
print(df)
## top 20 data
print(df.head(20))
# bottom 10
print(df.tail(10))
# size of data
print('Size of Data :', df.shape)
#filed info
print(df.columns)
# datatypes
print(df.dtypes)
# DATA CLEANING   --using unique
print(df['Item Fat Content'].unique())
#replace
df['Item Fat Content'] = df['Item Fat Content'].replace({'LF':'Low Fat', 'low fat':'Low Fat','reg':'Regular'})
#for see changes again print unique
print(df['Item Fat Content'].unique())
#buiness requrment
#KPI REQUIRMENTS
#total sales
total_sales=df['Sales'].sum()     
#avrage sales
avg_sales=df['Sales'].mean()
#no. of items sold
no_of_item_sold=df['Sales'].count()
# avrage rating
avg_rating=df['Rating'].mean()
#Display
print(f"Total Sales: {total_sales:,.0f}")  
print(f"Average Sales: {avg_sales:,.0f}")    
print(f"No of Items Sold: {no_of_item_sold:,.0f}")
print(f"Average Ratings: {avg_rating:,.0f}")   
#CHARTS REQUIRMENTS
# total sales by fat contents
sales_by_fat = df.groupby('Item Fat Content')['Sales'].sum()
plt.pie(sales_by_fat,labels=sales_by_fat.index,autopct='%.1f%%',startangle=90)
plt.title('Sales by fat Content')
plt.axis('equal')
plt.show()

# Total sales by Item Type
Sales_by_type=df.groupby('Item Type')["Sales"].sum().sort_values(ascending=False)

plt.figure(figsize=(10,6))
bars=plt.bar(Sales_by_type.index,Sales_by_type.values)
plt.xticks(rotation=90)
plt.ylabel('Item Type')
plt.title('Total Sales by Item Type')

for bar in bars:
    plt.text(bar.get_x() + bar.get_width() /2, bar.get_height(),f'{bar.get_height():,.0f}',ha='center',va='bottom', fontsize=8)

plt.tight_layout()
plt.show()

# Fat content by Outlet for Total Sales
grouped=df.groupby(['Outlet Location Type','Item Fat Content'])['Sales'].sum().unstack()
grouped=grouped[['Regular','Low Fat']]
ax=grouped.plot(kind='bar',figsize=(8,5),title='Outlet Tier by Item Fat Content')
plt.xlabel('Outlet Location Tier')
plt.ylabel('Total Sales')
plt.legend(title='Item Fat Content')
plt.tight_layout()
plt.show()

# Total Sales by Outlet Establishment
Sales_by_year=df.groupby('Outlet Establishment Year')['Sales'].sum().sort_index()
plt.figure(figsize=(9,5))
plt.plot(Sales_by_year.index,Sales_by_year.values,marker='o',linestyle='-')
plt.xlabel('Outlet Establishment Year')
plt.ylabel('Total Sales')
plt.title('Outlet Establishment')

for x,y in zip(Sales_by_year.index,Sales_by_year.values):
    plt.text(x,y,f'(y:,.0f)',ha='center',va='bottom',fontsize=8)
plt.tight_layout()
plt.show()

# Sales by Outlet Size
Sales_by_size = df.groupby('Outlet Size')['Sales'].sum()
plt.figure(figsize=(4,4))
plt.pie(Sales_by_size,labels=Sales_by_size.index,autopct='%1.1f%%',startangle=90)
plt.title('Outlet Size')
plt.tight_layout()
plt.show()

#Sales by Outlet Location
Sales_by_location=df.groupby('Outlet Location Type')['Sales'].sum().reset_index()
Sales_by_location=Sales_by_location.sort_values('Sales',ascending=False)
plt.figure(figsize=(8,3))          
ax=sns.barplot(x='Sales',y='Outlet Location Type',data=Sales_by_location)
plt.title('Total Sales By Outlet Location Type')
plt.xlabel('Total Sales')
plt.ylabel('Outlet Location Type')
plt.tight_layout()
plt.show()

