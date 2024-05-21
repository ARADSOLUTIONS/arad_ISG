# API
* POST https://ipg.aradly.com/api/v1/start_spend
* GET https://ipg.aradly.com/v1/?token={token}
* POST https://ipg.aradly.com/api/v1/transaction


## مرحله اول ارسال اطلاعات و دریافت توکن

````
POST
https://ipg.aradly.com/api/v1/start_spend

````
### Request

```json

{
  api_key: "my_api_key",
  amount: 1000,
  callback_url: "https://example.com/callback",
  mobile_number: "09123456789",
  order_no: "850e8400-e29b-41d4-a716-446655440000 ",
  details: "my decription",
  order_items: [
    {
      "price": 1320000,
      "product_uniqu_id": "550e8400-e29b-41d4-a716-446655440000 ",
      "quantity": 2,
      "item_type": "PRODUCT",
      "name": "دوره آموزش ۲۰ ساعته پایتون"
    },
    {
      "price": 20000,
      "quantity": 1,
      "item_type": "DISCOUNT",
      "name": "تخفیف جشنواره"
    },
    {
      "price": 132000,
      "quantity": 1,
      "item_type": "VAT",
      "name": "مالیات بر ارزش افزوده"
    }
  ]
}
```

### Response
```json

{
  status: "Success",
  token: "secret_token"
}
```
| itme type | Description          |
|-----------|----------------------|
 PRODUCT   | محصول                |
 VAT   | مالیات بر ارزش افرده |
 DISCOUNT   | تخفیف                |


| Request Body Field | Description                                                       | required |
|--------------------|-------------------------------------------------------------------|----------|
 api_key            |                                                                   | true     |
 amount                | مالیات بر ارزش افرده                                              | true     |
 callback_url           | باید با آدرس درگاه پرداخت تایید شده در وندار بر روی یک دامنه باشد | true     |
 mobile_number           | شماره موبایل کاربر                                                | false    |
 order_no           | شماره فاکتور شما                                                  | true     |
 details           | توضیحات حداکثر 255  char                                          | false    |
 order_items           | موارد سفارش داده شده                                              | false    |




# مرحله دوم  انتقال کاربر به صفحه درگاه پرداخت آراد

````
GET
https://ipg.aradly.com/v1/?token={token}
````


# مرحله سوم  انتقال کاربر به صفحه پذیرنده و بررسی تراکنش

```angular2html
POST 
https://ipg.aradly.com/api/v1/transaction
```
### Request

```json

{
    api_key: "my_api_key",
    token: "secret_token"
    
    
  }
```
### Response
```json

{
  status: "Success",
  amount: 10000,
  real_amount: 7000,
  wage: 3000,
  transaction_id: "dsds323sfds",
  order_no: "u12345", 
  detail: "my description" ,
  created_at: "2019-02-19 18:19:55",
  payed_at: "2019-02-19 18:21:13",
  status: "Success",
  status_description: " تراکنش موفق بوده است. "
}


```

| Status  | Description                 |
|---------|-----------------------------|
 Invalid | اطلاعات تراکنش نامعتبر است. |
 Success | تراکنش موفق بوده است.       |
 Expired | تراکنش منقضی شده است.       |
 Failed | تراکنش ناموفق بوده است.     |



| Field   | Description                                                                                                                                                                                      |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
 amount | مبلغ تراکنش که ممکن است با مبلغ تراکنشی که در مرحله اول ارسال کرده باشید متفاوت باشد. اگر کارمزد تراکنش بر عهده پرداخت کننده باشد. مبلغ کارمزد هم به مبلغ تراکنش ارسالی از سمت شما اضافه شده است |
 real_amount | مبلغی که بر اساس این تراکنش کیف پول شما بالا رفته است                                                                                                                                            |
 transaction_id | کارمزد تراکنش                                                                                                                                                                                    |
 order_no | شناسه یکتای تراکنش                                                                                                                                                                               |
 detail | توضیحات تراکنش                                                                                                                                                                                   |
 created_at | زمان ساخت لینک تراکشن                                                                                                                                                                            |
 payed_at | تاریخ و زمان پرداخت                                                                                                                                                                              |
 status | وضعیت تراکنش                                                                                                                                                                                     |
 status_description | توضیح وضعیت تراکنش                                                                                                                                                                               |

