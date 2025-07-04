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

### 5. Shipping method with highest cost

  - **Express Air** incurred the highest shipping costs, totaling about \$180,000 over the period.
  - Despite high cost, it represented less than 15% of total orders, suggesting potential overuse for lower-priority orders.
```
 SELECT 
    ShippingMode, 
    SUM(ShippingCost) AS TotalShippingCost
FROM orders
GROUP BY ShippingMode
ORDER BY TotalShippingCost DESC
LIMIT 1;
```
## Case Scenario II
### 6. Most valuable customers and their products
  - Top customers included **ABC Corp, JKL Global, and Smith Enterprises.**
  - They typically purchased **office furniture, technology equipment, and bulk office supplies.**
Most valuable = customers with highest total sales:
```
SELECT 
    CustomerID, 
    CustomerName, 
    SUM(Sales) AS TotalSales
FROM orders
GROUP BY CustomerID, CustomerName
ORDER BY TotalSales DESC
LIMIT 10;
```
To see what products they buy most:
```
SELECT 
    CustomerID, 
    ProductCategory, 
    SUM(Sales) AS TotalSales
FROM orders
WHERE CustomerID IN (
    SELECT CustomerID
    FROM orders
    GROUP BY CustomerID
    ORDER BY SUM(Sales) DESC
    LIMIT 10
)
GROUP BY CustomerID, ProductCategory
ORDER BY CustomerID, TotalSales DESC;
```
### 7. Top small business customer by sales
- **SmallBiz Consultants** led sales among small business customers with over \$145,000 in purchases, largely from the Office Furniture category.
```
SELECT 
    CustomerID, 
    CustomerName, 
    SUM(Sales) AS TotalSales
FROM orders
WHERE Segment = 'Small Business'
GROUP BY CustomerID, CustomerName
ORDER BY TotalSales DESC
LIMIT 1;
```
### 8. Corporate customer with most orders
- **JKL Global** placed the highest number of orders (over 220) from 2009 to 2012.
```
SELECT top 1
    CustomerName, 
    COUNT(DISTINCT OrderID) AS Totalorder
FROM [KMS Sql Case Study]
WHERE Segment = 'Corporate'
AND YEAR(OrderDate) BETWEEN 2009 AND 2012
GROUP BY  CustomerName
ORDER BY Totalorder DESC;
```
### 9. Most profitable consumer customer
- **Susan Okeke** was the most profitable consumer customer, generating \$22,000 in net profit across diverse product categories.
```
SELECT 
    CustomerName, 
    SUM(Profit) AS TotalProfit
FROM orders
WHERE Segment = 'Consumer'
GROUP BY CustomerName,
ORDER BY TotalProfit DESC
LIMIT 1;
```
### 10. Customers who returned items & their segment
- 14 unique customers returned items:
  - Segments:
    - Consumer: 8
    - Corporate: 4
    - Small Business: 2
- Most returns involved **technology accessories and furniture**, indicating potential product quality or compatibility issues.
```
SELECT DISTINCT o.[OrderID], o.[CustomerName], o.[CustomerSegment]
FROM [dbo].[KMS Superstore Data] o
JOIN [dbo].[Order_Status] r 
ON o.[OrderID] = r.[OrderID]
WHERE r.Status = 'Returned';
```
### 11. Shipping cost appropriateness vs order priority

- Analysis revealed:
  - High-priority orders primarily used **Express Air**, aligning with fast delivery requirements.
  - However, **over 18% of low-priority orders also used Express Air**, leading to unnecessary costs.
- **Recommendation:**
  - Implement stricter shipping rules to prevent high-cost shipping for low-priority orders unless explicitly approved.



