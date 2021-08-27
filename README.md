# TrustMark Schemes Integration

## Prerequisites

You must have a Scheme Portal account which will give you access to create an API Key to use with your requests.


## Key Concepts

As a scheme you will be able to submit business data of your registered members and the trades that they currently hold. Provision of this data will ensure that businesses are allocated a TrustMark Licence Number (TMLN), can be listed on the TrustMark website and can lodge work into the TrustMark Data Warehouse.

You can deregister a business therefore ensuring they can no longer lodge work and they fall out of your billing cycle.

## API Endpoints

All API endpoints require an x-api-key and tm-api-key Header values, tm-api-key can be accessed via your Scheme Portal login and your x-api-key will be issued by TrustMark.

### POST /SubmitBusiness

There is a single endpoint to submit the data for each business.

The business data should be self explanatory.

Trades data includes just the current active trades for the business with the `tradeCode` a value from the TrustMark data dictionary https://www.trustmark.org.uk/ourservices/data-warehouse/data-dictionary and the `certificateId` an optional value to provide the Registered Certificate Id for that trade (this includes any relevant regsitration or certification associated with the trade such as Gas Safe Registration No / MCS / PAS).

As part of the header for the body:

* `version` should be "2021-07-31"
* `membershipReference` should contain the unique your scheme reference for this business
* `action` must be a value from `CREATE_PENDING`, `SUBMIT`
* `schemeIdentity.trustmarkId` you will find this value as a pair for the API Key from the Scheme Portal

You will be able to see all the data submitted within your login in the Scheme Portal.

```json
{
  "version": "2021-07-31",
  "membershipReference": "T0004",
  "action": "SUBMIT",
  "schemeIdentity": {
    "trustmarkId": "api-30393a10-391b-4bf3-99e4-9748ea5ac4c8"
  },
  "business": {
    "registeredCompanyName": "TrustMark (2005) Limited",
    "address1": "Arena Business Centre, The Square",
    "address2": "Basing View",
    "town": "Basingstoke",
    "county": "Hampshire",
    "postCode": "RG21 4EB",
    "country": "England",
    "email": "data@trustmark.org.uk",
    "primaryContactNumber": "0333 555 1234",
    "website": "https://www.trustmark.org.uk/aboutus/contact-us",
    "phone": "0333 555 1234",
    "fax": "0333 555 1235",
    "registeredCompanyNumber": "05480144",
    "trades": [
      {
        "tradeCode": 22,
        "certificateId": "MYREF022"
      },
      {
        "tradeCode": 24,
        "certificateId": "ABC345"
      }
    ],
    "contacts": [
      {
        "firstName": "John",
        "lastName": "Doe",
        "email": "johndoe@trustmark.org.uk",
        "primaryContactNumber": "0333 555 1234",
        "jobTitle": "",
        "isFinanceContact": true,
        "isComplianceContact": false,
        "isOperationContact": true,
        "piNumber": ""
      }
    ],
    "notes": "Imported via API"
  }
}
```

Example response with status code 201 Created

```json
{
    "receipt": {
        "transactionDt": "2021-08-12T10:23:43.6216394Z",
        "schemeId": "testscheme",
        "receiptId": "rei-8be32e59-abc0-4a75-b198-7587d4832259",
        "outcomes": [
            "Updated",
            "Submitted"
        ],
        "schemeBusinessId": "sbu-aa7a6cf6-dcd8-4e98-908e-1dc4b0266368"
    },
    "request": {
        ...
    }
}
```

### PUT /DeregisterBusiness

Allows you to deregister a business so they are no longer active.

```json
{
  "version": "2021-07-31",
  "membershipReference": "T0004",
  "schemeIdentity": {
    "trustmarkId": "api-30393a10-391b-4bf3-99e4-9748ea5ac4c8"
  },
  "reason": "Suspension"
}
```

Example response with status code 201 Created

```json
{
    "receipt": {
        "transactionDt": "2021-08-13T10:47:52.3428798Z",
        "schemeId": "testscheme",
        "receiptId": "rei-b3ca93b0-1c69-40f1-9911-7abcaa63f693",
        "outcomes": [
            "Deregistered"
        ],
        "schemeBusinessId": "sbu-aa7a6cf6-dcd8-4e98-908e-1dc4b0266368"
    },
    "request": {
        ...
    }
}
```

### GET /Business/MembershipReference/{membershipReference}

Allow you to pull data back about the business by the membership reference you hold.

## Postman Examples

If you are uncertain about an integration or just want to test the API endpoints - there is a Postman link available https://www.getpostman.com/collections/d0b53b9f74050888eeda. Just update the variables in the API Collection with your own tm-api-key which you can get from the API section from your Scheme Portal login.

If you don't have Postman installed, you can get a copy here https://www.postman.com/
