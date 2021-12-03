# Direct Messages

## Create Direct Message

> Example Response Body:

```json
{
  "claimId": "c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
  "message": "Test message."
}
```

This route allows for sending a direct message to our Claim Specialists.

### HTTP Request

`POST https://xapi.lossexpress.com/claims/{claimId}/direct-messages`

### URL Parameters

Parameter | Description
--------- | -----------
claimId | The LossExpress UUID associated with the claim

### Request Body

This route accepts a JSON payload of an object comprising of:

Body Parameter | Description | Required?
-------------- | ----------- | ---------
message | The direct message to be sent to LossExpress | Y

## Embed Direct Messages

This allows for the above Direct Message functionality to be embedded into an iframe, serving the same experience we currently offer via our interface.

### Iframe Link

`https://{subdomain}.lossexpress.com/direct-messages/{claimId}/{oauthToken}`

### URL Parameters

Parameter | Description
--------- | -----------
subdomain | The subdomain associated with carrier/vendor
claimId | The LossExpress UUID associated with the claim
oauthToken | The token received via secret and clientId
