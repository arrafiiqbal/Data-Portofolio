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
