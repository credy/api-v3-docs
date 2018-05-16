![Logo](../assets/img/logo.png?raw=true)

# Introduction 

Credy is a company that specializes on the quality lead generation. 
This document further describes how to integrate API to send leads to Credy.
Should you have any questions, please do not hesitate to contact the IT manager at [it@credy.eu](mailto:it@credy.eu).

## General information

Production URL: [https://api.credy.eu/v3/](https://api.credy.eu/v3/)

Staging URL: [http://api.staging.credy.eu/v3/](http://api.staging.credy.eu/v3/)

API supports post fields, XML and JSON.

To get response error messages in spain, "Accept-Language" header’s value must be set to according ISO code. Ex: “id” or “id-ID“.

## Signing requests

Each request must be signed. Signature consists of api_key, timestamp and hash.

Hash is a SHA1 hash of the concatenation of timestamp, api_key and secret_key.

Allowed deviation for timestamp is UTC +/- 60 seconds.

*ex. sha1(timestamp + apiKey + secretKey)*

Api key and secret key will be assigned to you by Credy.

## Lead registration flow

1. Request is sent to /v3/customers

2. If request is successful, customer uuid must be stored

3. Request is sent to /v3/leads with previously stored customer uuid in query parameter "customer" *ex. /v3/leads?customer=a3ff7c20-e9e8-43a0-bfb7-71a5cf43b8d3*

# API methods

### POST /v3/customers

This method will create or update customer and return customer uuid.

## Request fields:

- **first_name**
    - type: string
    - required: true
    
- **last_name**
    - type: string
    - required: true
    
- **email**
    - type: string
    - required: true
    
- **phone**
    - type: string
    - required: true
    
- **personal_id**
    - type: string
    - required: true
    
- **phone**
    - type: string
    - required: true

- **phone2**
    - type: string
    - required: false
    
- **spouse_personal_id**
    - type: string
    - required: when `marital_status` is 'MARRIED'
    
- **spouse_first_name**
    - type: string
    - required: when `marital_status` is 'MARRIED'
    
- **spouse_last_name**
    - type: string
    - required: when `marital_status` is 'MARRIED'

- **spouse_split_agreement**
    - type: bool
    - required: when `marital_status` is 'DIVORCED'
    
- **mother_first_name**
    - type: string
    - required: true
    
- **mother_last_name**
    - type: string
    - required: true
    
- **religion**
    - type: ENUM(string)
    - required: true
    - possible values:
        - CHRISTIAN
        - CATHOLIC
        - MUSLIM
        - HINDU
    
- **nationality**
    - type: string
    - required: true
    
- **marital_status**
    - type: ENUM(string)
    - required: true
    - possible values:
        - SINGLE
        - DIVORCED
        - MARRIED
        - WITH_PARTNER
        - WIDOW
    
- **dependant_count**
    - type: int
    - required: true
    
- **education**
    - type: ENUM(string)
    - required: true
    - possible values:
        - NO_EDUCATION
        - BASIC_SCHOOL
        - HIGH_SCHOOL
        - INDUSTRIAL_SCHOOL
        - BACHELOR
        - MASTER
        - PHD
   
- **housing_type**
    - type: ENUM(string)
    - required: true
    - possible values:
        - RENTED_ROOM
        - RENTED_APARTMENT
        - RENTED_HOUSE
        - OWN_APARTMENT
        - OWN_HOUSE
        - WITH_PARENTS
    
- **monthly_expenses**
    - type: int
    - required: true
    
- **monthly_utility_expenses**
    - type: int
    - required: true
    
- **address**
    - type: PrimaryAddress
    - required: true
    
- **lives_at_registered_address**
    - type: bool
    - required: true
    
- **secondary_address**
    - type: PrimaryAddress
    - required: when `lives_at_registered_address` is false
    
- **employer_address**
    - type: ContactAddress
    - required when `occupation` is in ('EMPLOYED_INDEFINITE_PERIOD', 'EMPLOYED_SPECIFIED_PERIOD', 'WRITTEN_CONTRACT_OR_ORDER', 'MILITARY_POLICE')

- **tax_id_number**
    - type: string
    - required: true

- **employee_id**
    - type: string
    - required: required when `occupation` is in ('EMPLOYED_INDEFINITE_PERIOD', 'EMPLOYED_SPECIFIED_PERIOD', 'WRITTEN_CONTRACT_OR_ORDER', 'MILITARY_POLICE')
        
- **employer**
    - type: string
    - required: required when `occupation` is in ('EMPLOYED_INDEFINITE_PERIOD', 'EMPLOYED_SPECIFIED_PERIOD', 'WRITTEN_CONTRACT_OR_ORDER', 'MILITARY_POLICE')
    
- **current_job_position**
    - type: ENUM(string)  
    - required: required when `occupation` is in ('EMPLOYED_INDEFINITE_PERIOD', 'EMPLOYED_SPECIFIED_PERIOD', 'WRITTEN_CONTRACT_OR_ORDER', 'MILITARY_POLICE')
    - possible values:  
        - CONTRACT
        - STAFF
        - SUPERVISOR
        - JUNIOR_MANAGEMENT
        - MIDDLE_MANAGEMENT
        - SENIOR_MANAGEMENT
        
- **contact_person_address**
    - type: ContactAddress
    - required: true
    
- **contact_person_first_name**
    - type: string
    - required: true
    
- **contact_person_last_name**
    - type: string
    - required: true

- **contact_person_phone**
    - type: string
    - required: true
    
- **occupation**
    - type: ENUM(string)
    - required: true
    - possible values:
        - EMPLOYED_INDEFINITE_PERIOD
        - EMPLOYED_SPECIFIED_PERIOD
        - WRITTEN_CONTRACT_OR_ORDER
        - MILITARY_POLICE
        - ECONOMIC_ACTIVITY
        - UNEMPLOYED
        - STUDENT
        - PENSIONER1
        - OTHER
    
- **neto_income**
    - type: int
    - required: true
    
- **bank_name**
    - type: string
    - required: true
    
- **bank_account**
    - type: string
    - required: true
  
## Types  

### PrimaryAddress
- **city**
    - type: string
    - required: true
    
- **street**
    - type: string
    - required: true
    
- **house_number**
    - type: string
    - required: true
    
- **postal_code**
    - type: string
    - required: true
    - pattern: '/^[0-9]{5}$/'
    
- **village**
    - type: string
    - required: true
    
- **province**
    - type: string
    - required: true
    
- **district**
    - type: string
    - required: true
    
- **rt**
    - type: string
    - required: true
    
- **rw**
    - type: string
    - required: true

### ContactAddress  
- **city**
    - type: string
    - required: true
     
- **street**
    - type: string
    - required: true
    
- **house_number**
    - type: string
    - required: true
    
- **postal_code**
    - type: string
    - required: true
    - pattern: '/^[0-9]{5}$/'
  
### JSON examples for /v3/customers

**POST /v3/customers**

| Headers      |                  |
|--------------|------------------|
| Content-Type | application/json |
| Accept       | application/json |

Body:
```json
//TODO
```

**Successful response for /v3/customers**

Status code: 2xx

Response:
```json
// TODO
```

**Failed response for /v3/customers**

Status code: 422

Response:
```json
[
// TODO
]
```

### XML examples for /v3/customers

**POST /v3/customers**

| Headers      |                  |
|--------------|------------------|
| Content-Type | application/xml  |
| Accept       | application/xml  |


Body:
```xml
// TODO
```

**Successful response for /v3/customers**

Status code: 2xx

Response:
```xml
// TODO
```

**Failed response for /v3/customers**

Status code: 422

Response:
```xml
// TODO
```

### POST /v3/leads?customer={customer_uuid} *

Creates lead for existing customer.

* Replace customer_uuid placeholder with received uuid value from POST /v3/customers response.

- **loan_sum**
    - type: int
    - required: true
    - description: Amount of credit requested by customer

- **loan_period**
    - type: int
    - required: true
    - description: Term for credit requested

- **ip_address**
    - type: string
    - required: true
    - description: IP address of customer
    
- signature
    - type: Signature
    - required: true
    - description: Request signature

## Types

### Signature
- **timestamp**
    - type: int
    - required: true
    - description: unix timestamp, must be UTC +/- 60 seconds
    
- **api_key**
    - type: string
    - required: true
    - description: API key
    
- **hash**
    - type: string
    - required: true
    - description sha1 concatenation of timestamp, api key and secret key

### JSON examples for /v3/leads

**POST /v3/leads?customer=bbf18703-1f8d-5c8a-a83b-9433f003807f**

| Headers      |                  |
|--------------|------------------|
| Content-Type | application/json |
| Accept       | application/json |


Body:

```json
{
// TODO
}
```

**Successful response for /v3/leads**

Status code: 2xx

Response:
```json
{
// TODO
}
```

**Example failed response for /v3/leads**

Status code: 422

Response:
```json
[
   {
      "field":"loan_sum",
      "message":"Loan Sum cannot be blank."
   }
]
```

### XML examples for /v3/leads

| Headers      |                  |
|--------------|------------------|
| Content-Type | application/xml  |
| Accept       | application/xml  |


**POST /v3/leads?customer=318d4a7b-b39e-58f2-a413-caefbe1615b7**

Body:
```xml
// TODO
```

**Successful response for /v3/leads?customer=318d4a7b-b39e-58f2-a413-caefbe1615b7**

Status code: 2xx

Response:
```xml
// TODO
```

**Failed response for /v3/leads?customer=318d4a7b-b39e-58f2-a413-caefbe1615b7**

Status code: 422

Response:
```xml
// TODO
```

# Revisions

- 1.0.0 (2018-05-16): first publish
