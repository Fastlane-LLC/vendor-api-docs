---
title: LossExpress Vendor API

includes:
  - activity_feed
  - carriers
  - claims
  - direct_messages
  - documents
  - letter_of_guarantee
  - payments
  - settlement_counters
  - errors

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

# Authentication

Our Vendor API utilizes a standard OAuth 2.0 Client Credentials flow, where the external server authenticates itself using a provided client ID and client secret and is provided an access token that will be used on every request.

Once a token has been received, every request is expected to have a header that looks like the following:

`Authorization: Bearer exampletokenbutreplacewithyourown`

### HTTP Request
`POST https://exapi.lossexpress.com/oauth/token`

<aside class="notice">
You must replace <code>exampletokenbutreplacewithyourown</code> with the access token provided.
</aside>
