# 📝 Complete Technical Process

## **Objective**

- Display annual and monthly summaries of purchases, sales, profits, and units sold  
- Analyze profits and sales:  
    - **By market/country**: Identify profitability per region and business opportunities  
    - **By category/subcategory**: Locate areas for improvement and adapt marketing campaigns  

- **Year-over-year comparison** of sales, costs, profit, and units sold  
- Detailed analysis of business **growth** to detect improvements/declines in markets and products  

## **Tools**  
- **Power Query**: Data transformation and cleaning  
- **DAX**: Measures like `Margin = DIVIDE([SUM_Profit],[SUM_Sales],0)`  
- **Excel & Python**: Data integration  

## **Key Steps**  

### **1. Data Cleaning (Power Query)**  
- Changed data types for sales, profit, and shipping cost columns to USD currency (US regional settings)  
- Converted Order Date and Ship Date from text to date using US regional format  
- Created calculated column: `Cost = Sales - Profit`  
- Removed unnecessary columns (postal code)  

### **2. Data Modeling**  
- **Relationships**: Established 1:* relationship between "DateTable" and "Global-Superstore[Order Date]"  
- **Hierarchies**: Created "Date Hierarchy": Year → Month  

### **3. DAX Measures (Advanced Analysis)**  

`Margin = DIVIDE([SUM_Profit],[SUM_Sales],0)`  

- **Purpose**: Calculate profit margin relative to cost  

`Profit PY = CALCULATE(SUM('Global-Superstore'[Profit]), SAMEPERIODLASTYEAR('DateTable'[Date]))`  

- **Purpose**: Calculate prior year profit (also created for "Cost", "Sales", and "Quantity")  

`Profit Growth % Improved = 
VAR ProfitCurrent = SUM('Global-Superstore'[Profit])
VAR ProfitPrevious = [Profit PY]
VAR Growth = DIVIDE(ProfitCurrent - ProfitPrevious, ProfitPrevious, BLANK())
RETURN
    IF(
        ISBLANK(ProfitCurrent) || ISBLANK(ProfitPrevious),
        BLANK(),
        IF(
            Growth >= 0,
            FORMAT(Growth, "+0.0%"),  
            FORMAT(Growth, "-0.0%")   
        )
    )`  

- **Purpose**: Display YoY growth with +/- formatting (also implemented for "Cost", "Sales", "Quantity")  

### **4. Visualization**  

- **Summary (Page 1)**: Overview with KPIs (Sales, Margin, Costs, Units) + YoY comparison  
- **Products (Page 2)**: Analysis by categories/subcategories (sales, margin, costs)  
- **Logistics (Page 3)**: Market/country analysis + sales by order priority  

**Advanced Interface**  
- Year selection buttons  
- Accessible index  
- Dropdown filters for charts  

### **5. Insights**  

#### **Annual Growth**  
- Highest growth in 2013: +32.4% profit vs 2012  
- In 2014 (latest record):  
  - Profits: +23.9% vs 2013  
  - Sales: +26.6% vs 2013  
- **Trend**: Consistent yearly growth  

#### **Markets**  
- **Top 2014**:  
  - APAC:  
    - Sales: $1,209,199  
    - Profit: $140,454  
  - EU:  
    - Sales: $1,042,204  
    - Profit: $128,944  
- **Highest margin**: Canada (despite lower sales volume)  

#### **Products**  
- **Issue**: "Tables" subcategory (Furniture) with 12.5% losses (-$30,546)  
- **Top performer**:  
  - "Technology" category ($234,928 profit)  
  - "Copiers" subcategory ($104,049 profit, 18.9% margin)  
