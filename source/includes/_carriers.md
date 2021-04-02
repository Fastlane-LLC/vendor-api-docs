# Carriers

In order to create a claim using the LossExpress API, all vendors must pass in a LossExpress UUID associated with that carrier in our system. You can  manage carriers in our system using the routes in this section.

## Create Carrier

> Example Response:

```json
{
  "carrierId": "150ae9da-9222-4ca5-43fe-fe1dc650fa0f"
}
```

This route allows vendors to create a carrier in our system and associate it with their account.

<aside class="notice">
Carriers with the same business name are treated as the same entity in our system to prevent duplication and to allow better service tracking.
</aside>

### HTTP Request

`POST https://exapi.lossexpress.com/carriers`

### Request Body

This route accepts a JSON payload of an object comprising of:

Body Parameter | Description | Required?
-------------- | ----------- | ---------
businessName | The name the carrier places on documentation related to claims | Y

## View Carriers

> Example Response:

```json
{
  "carriers": [
    {
      "carrierId": "150ae9da-9222-4ca5-43fe-fe1dc650fa0f",
      "carrierName": "LossExpress Test Carrier"
    }
  ]
}
```