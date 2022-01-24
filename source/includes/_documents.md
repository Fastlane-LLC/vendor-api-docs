# Documents

## Document Types

Some documents are required in order to receive a payoff quote. The accepted (case-insensitive) standard document types for a claim are:

Description | Required
--------- | -----------
settlement breakdown | Y (if carrier configuration does not allow combined documents)
valuation report | Y (if carrier configuration does not allow combined documents)
total loss evaluation | Y (if valuation report is not added, and carrier configuration does not allow combined documents)
valuation report & settlement breakdown | Y (if carrier configuration allows combined documents)
cause of loss | N
cause of loss statement | N
police report | N
declaration page | N
proof of payment | N
other | N



## Add Document to Claim

> Example Response Body:

```json
{
  "documentUrl": "https://xapi.lossexpress.com/documents/c30ae9da-9222-4de5-81fe-fe1ac590fa0f",
}
```

This route allows for adding documents to claims

### HTTP Request

`POST https://xapi.lossexpress.com/claims/documents/{claimId}`

### Request Body

This route accepts a JSON payload of an object comprising of:

Parameter | Description
--------- | -----------
document | A base64-encoded file string. Acceptable formats: `pdf`, `jpeg`, `jpg`, `tiff`, `gif`, `png`
documentUrl | A url that points to a downloadable file of the above formats
description | A brief description of the document (eg, Settlement Breakdown)



## Fetch Document

This route allows for fetching documents from our system as PDFs. Any activity that references a document in our system will include a URL for this request.

### HTTP Request

`GET https://xapi.lossexpress.com/documents/{documentId}`
