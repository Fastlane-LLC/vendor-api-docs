---
title: LossExpress Vendor API

includes:
  - activity_feed
  - carriers
  - claims
  - direct_messages
  - documents
  - lenders
  - letter_of_guarantee
  - payoff_data
  - orders
  - payments
  - settlement_counters
  - errors

language_tabs:
  - js: JavaScript
  - json:JSON

search: true

code_clipboard: true
---

# Introduction

Welcome to the LossExpress Vendor API! This API is designed for use by varying third-party vendors that wish to trigger processing events within LossExpress and view the results of those events, on behalf of insurance carriers.

When going through the integration process with your organization’s systems and LossExpress, it’s important to keep in mind the idea at the core of the LossExpress product and underlying APIs: we act simply as a conduit of information, from carriers to lenders.

As a result, we have done our best when building out this API to give the power to make decisions to the carrier (where that power belongs), not ourselves, and recommend that our vendors do their best to leave decisioning to the carriers whenever possible.

<aside class="notice">
Not all Vendors may have access to all routes or activity types within our Vendor API. Please consult initial onboarding material for a full detailing of any limitations that are in place for your Vendor account.
</aside>

# Testing

Upon receiving authentication credentials, vendors are initially put into a testing phase. This is a sandbox environment where you can create and manipulate claims - and make mistakes - without affecting production data or processes. Once testing has been thoroughly completed, let us know and we will take you out of the testing phase.

### Test VINs

During testing, you can create claims with these VINs to trigger actions which emulate ones that LossExpress or lenders may make for a claim.

|      **VIN**      |                     **Action**                     |
|-------------------|----------------------------------------------------|
| JHMGE8H35DC074448 |  LossExpress sends a direct message for the claim  |
| JHMGE8H35DC075558 |     Payoff information is attached to the claim    |
| JHMGE8H35DC076668 |    Loss Express updates the claim's information    |
| 1GCHC29U62E142306 |        Loss Express calls the claim's lender       |
| 1GCHC29U62E147776 |    Loss Express attaches a document to the claim   |
| JHMFA36246S006140 | LossExpress sends a document to the claim's lender |
| JN1CV6AP7CM621670 |   A letter of guarantee is attached to the claim   |
| 1FAHP3EN7BW114135 |       The lender countered for the settlement      |

# Authentication

Our Vendor API utilizes a standard OAuth 2.0 Client Credentials flow, where the external server authenticates itself using a provided client ID and client secret and is provided an access token that will be used on every request.

Once a token has been received, every request is expected to have a header that looks like the following:

`Authorization: Bearer exampletokenbutreplacewithyourown`

## HTTP Request
`POST https://xapi.lossexpress.com/oauth/token`

This route is for generating OAuth tokens that can be used for all other requests.

### Request Body

This route accepts a JSON payload of an object comprising of:

Body Parameter | Description
-------------- | -----------
clientId | Your unique identifier in Loss Express systems
secret | An OAuth secret provided by Loss Express

<aside class="notice">
You must replace <code>exampletokenbutreplacewithyourown</code> with the access token provided.
</aside>
