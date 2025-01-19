## Amazon India Sales Data Analysis  

### Overview  
Independently analyzed a dataset containing **128,976 rows and 24 columns** from Amazon India sales data.  
Performed **data cleaning**, **exploratory data analysis (EDA)**, and created an **interactive dashboard** to uncover actionable insights and recommendations.  

### Key Insights and Recommendations  

#### 1. **Revenue and Order Volume**  
   - **Total Revenue:** ₹71.05M INR.  
   - **Total Orders:** 102,000.  
   - **Average Order Value (AOV):** ₹694 INR.  

   **Recommendation:**  
   - Work with the **marketing** and **product** departments to implement cross-selling and upselling strategies, such as bundling related products or offering discounts for larger orders, to increase AOV.  

#### 2. **Top-Performing Categories**  
   - Three categories—**Item Category Set, Kurta, and Western Dress**—contribute to over **90% of total revenue**, indicating their high popularity among Indian users.  

   **Recommendation:**  
   - Collaborate with the **merchandising** and **inventory management** teams to ensure sufficient stock and expand the product range within these high-performing categories.  
   - Partner with the **marketing** team to run targeted campaigns promoting these popular categories.  

#### 3. **High Cancellation Rate**  
   - The overall cancellation rate is **~14%**, consistently distributed across states and product categories.  
   - **99.84% of canceled orders** were from non-promotional purchases, highlighting a potential area for intervention.  

   **Recommendation:**  
   - Engage the **customer support** and **logistics** teams to investigate reasons for cancellations in non-promotional orders, such as delivery delays or customer dissatisfaction.  
   - Collaborate with the **promotions** and **marketing** teams to introduce incentives for non-promotional purchases, such as loyalty points or free shipping.  

### Tools and Technologies  
- **Programming Languages:** SQL  
- **Dashboard Tool:** Bigquery, Looker Studio

---  


* Original dataset : [E-Commerce Sales Dataset](https://www.kaggle.com/datasets/thedevastator/unlock-profits-with-e-commerce-sales-data)
* Supporting dataset : [Pincodes-India](https://www.kaggle.com/datasets/devsb13/pincodesindia)
* Cleaned dataset : [Cleaned Amazon India Sales Dataset](https://www.kaggle.com/datasets/iqbal911arrafii/cleaned-amazon-india-sales-dataset)  

The process of cleaning the dataset involves using supporting dataset (India pincode) to validate ship_city and ship_state column of the original dataset. As well as changing the datatype into reasonable one and parsing the date format.

The cleaning process is simple and starightforward as follows : 

```sql
SELECT  
  trx.index,
  trx.order_id,
  date(PARSE_DATE('%m-%d-%y', 
    REGEXP_REPLACE(
      REGEXP_REPLACE(date, r'(\d+)/(\d+)/(\d+)', r'\1-\2-\3'), 
      r'(\d{1,2})-(\d{1,2})-(\d{2})', r'\1-\2-\3'
    )
  )) AS parsed_date,
  lower(trx.status) as status,
  lower(trx.fulfillment) as fulfillment,
  lower(trx.sales_channel) as sales_channel,
  lower(trx.ship_service_level) as ship_service_level,
  trx.style,
  trx.sku,
  lower(trx.category) as category,
  trx.size,
  trx.asin,
  lower(trx.courier_status) as courier_status,
  cast(trx.qty as int64) as qty,
  trx.currency,
  cast(trx.amount as float64) as amount,
  pin.District as ship_city,
  pin.StateName as ship_state,
  trx.ship_postal_code,
  trx.ship_country,
  trx.promotion_ids,
  trx.is_b2b,
  lower(trx.fulfilled_by) as fulfilled_by,
  trx.unnamed_cols
FROM `amazon_india_sales_dataset` trx
LEFT JOIN `india_pincode_dataset` pin 
  ON cast(trx.ship_postal_code as int64) = pin.Pincode
```
