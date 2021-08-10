# TrustMark Schemes Integration

## Prerequisites

You must have a Scheme Portal account which will give you access to create an API Key to use with your requests.


## Key Concepts

As a scheme you will be able to submit business data of your registered members and the trades that they currently hold. Provision of this data will ensure that businesses are allocated a TrustMark Licence Number (TMLN) and can lodge measures of work delivered.

You can deregister a business therefore ensuring they can no longer lodge work and they fall out of your billing cycle.

## API Endpoints

All API endpoints require an x-api-key and tm-api-key Header values, tm-api-key can be accessed via your Scheme Portal login and your x-api-key will be issued by TrustMark.

### POST /SubmitBusiness

There is a single endpoint to submit the data for each business.

The business data should be self explanatory.

Trades data includes just the current active trades for the business with the `tradeCode` a value from the TrustMark data dictionary https://www.trustmark.org.uk/ourservices/data-warehouse/data-dictionary and the `certificateId` an optional value to provide the Registered Certificate Id.

As part of the header for the body:

* `version` should be "2021-07-31"
* `membershipReference` should contain the unique your scheme reference for this business
* `action` must be a value from `CREATE_PENDING`, `SUBMIT`
*  `schemeIdentity.trustmarkId` you will find this value as a pair for the API Key from the Scheme Portal

You will be able to see all the data submitted within your login in the Scheme Portal.

```json
{
  "version": "string",
  "membershipReference": "string",
  "action": "string",
  "schemeIdentity": {
    "trustmarkId": "string"
  },
  "business": {
    "registeredCompanyName": "string",
    "address1": "string",
    "address2": "string",
    "town": "string",
    "county": "string",
    "postCode": "string",
    "country": "string",
    "email": "string",
    "primaryContactNumber": "string",
    "website": "string",
    "phone": "string",
    "fax": "string",
    "registeredCompanyNumber": "string",
    "trades": [
      {
        "tradeCode": 0,
        "certificateId": "string"
      }
    ],
    "contacts": [
      {
        "firstName": "string",
        "lastName": "string",
        "email": "string",
        "primaryContactNumber": "string",
        "jobTitle": "string",
        "isFinanceContact": true,
        "isComplianceContact": true,
        "isOperationContact": true,
        "piNumber": "string"
      }
    ],
    "tmln": "string",
    "notes": "string"
  }
}
```

### DELETE /DeregisterBusiness

Allows you to deregister a business so they are no longer active.

```json
{
  "membershipReference": "string",
  "reason": "string",
  "deregisterDate": "2021-08-10T21:04:45.645Z"
}
```

### GET /Business/MembershipReference/{membershipReference}

Allow you to pull data back about the business by the membership reference you hold.

## Postman Examples

If you are uncertain about an integration or just want to test the API endpoints - there is a Postman link available https://www.getpostman.com/collections/d0b53b9f74050888eeda. Just update the variables in the API Collection with your own tm-api-key which you can get from the API section from your Scheme Portal login.
