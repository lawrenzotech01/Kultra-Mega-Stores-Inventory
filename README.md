# Kultra-Mega-Stores-Inventory
Kultra Mega Stores (KMS), headquartered in Lagos, specialises in office supplies and  furniture. Its customer base includes individual consumers, small businesses (retail), and  large corporate clients (wholesale) across Lagos, Nigeria. 
## Project Scope

This analysis covers:

- Sales trends
- Customer segmentation
- Shipping costs
- Product performance
- Recommendations for revenue improvement
  
## Data
- File: KMS Sql Case Study.csv
- Period: 2009 – 2012
  
## Tools Used
- SQL

  ## Case Scenario I
1. **Product category with highest sales**
2. **Top 3 and bottom 3 regions in sales**
3. **Total sales of appliances in Ontario**
4. **Recommendations to increase revenue from bottom 10 customers**
5. **Shipping method with highest cost**

## Case Scenario II
6. **Most valuable customers and their products**
7. **Top small business customer by sales**
8. **Corporate customer with most orders**
9. **Most profitable consumer customer**
10. **Customers who returned items & their segment**
11. **Shipping cost appropriateness vs order priority**

## Case Scenario I
### 1. Product category with highest sales
- **Office Supplies** was the top-performing category, generating total sales of approximately \$2.4 million over the 4-year period.
```
SELECT 
    ProductCategory, 
    SUM(Sales) AS TotalSales
FROM orders
GROUP BY ProductCategory
ORDER BY TotalSales DESC
LIMIT 1;
```
### 2. Top 3 and bottom 3 regions in sales
- **Top 3 regions:**
  - Ontario
  - Quebec
  - British Columbia
  ```
  SELECT 
    Region, 
    SUM(Sales) AS TotalSales
  FROM [KMS Sql Case Study]
  GROUP BY Region
  ORDER BY TotalSales DESC
  LIMIT 3;
  
- **Bottom 3 regions:**
  - Manitoba
  - Yukon
  - Northwest Territories
 ```
  SELECT 
    Region, 
    SUM(Sales) AS TotalSales
  FROM [KMS Sql Case Study]
  GROUP BY Region
  ORDER BY TotalSales ASC
  LIMIT 3;
```
### 3. Total sales of appliances in Ontario
- Total sales for **Appliances in Ontario** amounted to approximately \$320,000 over 4 years.
 ``` 
  SELECT 
    SUM(Sales) AS ApplianceSales_Ontario
FROM [KMS Sql Case Study]
WHERE 
    ProductCategory = 'Appliances'
    AND Region = 'Ontario'
Group by Region;
```
  ### 4. Recommendations to increase revenue from bottom 10 customers
- The bottom 10 customers collectively contributed less than \$8,000 in sales.
- They primarily purchased low-margin items such as paper products.
- **Recommendations:**
  - Offer loyalty discounts for higher-value purchases.
  - Promote bundled office furniture packages.
  - Target these customers with personalized marketing highlighting high-margin products.
```
SELECT 
    CustomerID, 
    CustomerName, 
    SUM(Sales) AS TotalSales
FROM orders
GROUP BY CustomerID, CustomerName
ORDER BY TotalSales ASC
LIMIT 10;
```
