# Orders

## Fetch Order Types

> Fetch Order Types Request Example Response Body 

```json
{
  "orderTypes": ["...array of current order types"],
  "success": true
}
```

This route returns an array of strings, which consist of the current types of orders that can be requested on a claim.

### HTTP Request

`GET https://xapi.lossexpress.com/claims/order-types`

## Create Order(s) Request

> Create Order(s) Request Example Response Body:

```json
{
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "orders": [
    {
      "orderId": "3e8c38f3-f4ef-4414-88da-1793d25ef6f0", 
      "type": "Repo Affidavit"
    }, 
    {
      "orderId": "77462941-7b6c-4bd0-9749-588c32654864",
      "type": "Copy of Title"
    }
  ],
  "success": true
}
```

This route will create a new order on a claim. 

### HTTP Request

`POST https://xapi.lossexpress.com/claims/{claimId}/orders`

### URL Parameters

Parameter | Description
--------- | -----------
claimId | The LossExpress UUID associated with the claim

### Request Body

This route accepts a JSON payload of an object comprising of:

Body Parameter | Description | Required?
-------------- | ----------- | ---------
types | An array containing strings with types of orders to be created | Y


## Cancel Order(s) Request

> Cancel Order(s) Request Example Response Body:

```json
{
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "success": true
}
```

### HTTP Request

`DELETE https://xapi.lossexpress.com/claims/{claimId}/cancel-orders`

### URL Parameters

Parameter | Description
--------- | -----------
claimId | The LossExpress UUID associated with the claim

### Request Body

This route accepts a JSON payload of an object comprising of:

Body Parameter | Description | Required?
-------------- | ----------- | ---------
orderIds | An array containing orderIds of orders to be cancelled | Y