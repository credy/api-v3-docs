![Logo](../assets/img/logo.png?raw=true)

# Introduction 

Credy is a company that specializes on the quality lead generation. 
This document further describes how to integrate API to send leads to Credy.
Should you have any questions, please do not hesitate to contact the IT manager at [it@credy.eu](mailto:it@credy.eu).

## General information

Production URL: [https://api.credy.eu/v3/](https://api.credy.eu/v3/)

Staging URL: [http://api.staging.credy.eu/v3/](http://api.staging.credy.eu/v3/)

API supports post fields, XML and JSON.

To get response error messages in spanish, "Accept-Language" header’s value must be set to “es”.

## Signing requests

Each request must be signed. Signature consists of api_key, timestamp and hash.

Hash is a SHA1 hash of the concatenation of timestamp, api_key and secret_key.

Allowed deviation for timestamp is UTC +/- 60 seconds.

*ex. sha1(timestamp + apiKey + secretKey)*

Api key and secret key will be assigned to you by Credy.

## Lead registration flow

1. Request is sent to /v3/customers

2. If request is successful, customer uuid must be stored (uuid is a field in response)

3. Request is sent to /v3/leads with previously stored customer uuid in query parameter "customer" *ex. /v3/leads?customer=a3ff7c20-e9e8-43a0-bfb7-71a5cf43b8d3*

# API methods

### POST /v3/customers

This method will create or update customer and return customer uuid.

| Field                           | Validations                                                                                                                                         | Description                                                |
|---------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| first_name                      | Required <br /> string (max 255), must match [regex](#regular-expressions)                                                                          | First name of customer                                     |
| last_name                       | Required <br /> string (max 255), must match [regex](#regular-expressions)                                                                          | Last name of the customer                                  |
| second_last_name                | Required <br /> string (max 255), must match [regex](#regular-expressions)                                                                          | Second last name of the customer                           |
| nationality                     | Required <br /> string (max 255), must match [countries](#countries)                                                                                | Nationality of the customer                                |
| phone                           | Required <br /> must be valid phone number                                                                                                          | Customer mobile phone number                               |
| phone_plan                      | Required <br /> format: PREPAID, CONTRACT                                                                                                           | Phone plan of the customer                                 |
| email                           | Required <br /> valid email address                                                                                                                 | Email address                                              |
| personal_id                     | Required <br /> must be valid DNI                                                                                                                   | DNI                                                        |
| bank_account                    | Optional <br /> must be valid IBAN, must match [regex](#bank-account-regexp)                                                                        | Bank account                                               |
| occupation                      | Required <br /> for allowed values see [Addendum A](#addendum-a---enums)                                                                            | Occupation                                                 |
| housing_type                    | Required <br /> for allowed values see [Addendum A](#addendum-a---enums)                                                                            | Housing type                                               |
| marital_status                  | Required <br /> for allowed values see [Addendum A](#addendum-a---enums)                                                                            | Marital status                                             |
| education                       | Required <br /> for allowed values see [Addendum A](#addendum-a---enums)                                                                            | Education                                                  |
| address                         | Required <br /> must match [regex](#regular-expressions)                                                                                            | Real address of the customer                               |
| address[city]                   | Required <br /> must match [regex](#regular-expressions)                                                                                            | City                                                       |
| address[street]                 | Required <br /> free text                                                                                                                           | Street                                                     |
| address[house_number]           | Required <br /> must match [regex](#regular-expressions)                                                                                            | House number                                               |
| address[flat_number]            | Optional <br /> free text                                                                                                                           | Flat number                                                |
| address[postal_code]            | Required <br /> must match [regex](#regular-expressions)                                                                                            | Postal index                                               |
| lives_at_registered_address     | Optional <br /> boolean; defaults to true                                                                                                           | Is customer’s real address the same as registered address? |
| secondary_address               | Required <br /> Object                                                                                                                              | Registered address of the customer                         |
| secondary_address[city]         | Required <br /> free text                                                                                                                           | City                                                       |
| secondary_address[street]       | Required <br /> free text                                                                                                                           | Street                                                     |
| secondary_address[house_number] | Required <br /> must match [regex](#regular-expressions)                                                                                            | House number                                               |
| secondary_address[flat_number]  | Optional <br /> free text                                                                                                                           | Flat number                                                |
| secondary_address[postal_code]  | Required <br /> must match [regex](#regular-expressions)                                                                                            | Postal index                                               |
| current_job_position            | Required when occupation in list EMPLOYED_INDEFINITE_PERIOD, EMPLOYED_SPECIFIED_PERIOD, WRITTEN_CONTRACT_OR_ORDER <br /> free text                  | Current job position                                       |
| neto_income                     | Required <br /> numeric                                                                                                                             | Net income                                                 |
| monthly_expenses                | Required <br /> numeric                                                                                                                             | Monthly expenses                                           |
| remuneration_deadline           | Required when occupation != UNEMPLOYED; format: YYYY-MM-DD                                                                                          | Remuneration deadline                                      |
| employed_since                  | Required when occupation in list EMPLOYED_INDEFINITE_PERIOD, EMPLOYED_SPECIFIED_PERIOD, WRITTEN_CONTRACT_OR_ORDER <br /> format: YYYY-MM-DD         | Employed since date                                        |
| bad_credit_history              | Required <br /> boolean <br /> defaults to false                                                                                                    | Does customer have bad credit history?                     |
| dependant_count                 | Required <br /> integer                                                                                                                             | Dependant count                                            |
| signature                       | Required <br /> Object                                                                                                                              | Request signature                                          |
| signature[timestamp]            | Required <br /> unix timestamp – must be UTC +/- 60 seconds                                                                                         | Timestamp when request is made                             |
| signature[api_key]              | Required                                                                                                                                            | Api key                                                    |
| signature[hash]                 | Required                                                                                                                                            | A sha1 concatenation of timestamp, api key and secret key  |
| agree_electronic_services       | Required <br /> Boolean                                                                                                                             | Does customer agree with electronic services               |
| agree_personal_data_protection  | Required <br /> Boolean                                                                                                                             | Does customer agree with personal data protection ?        | 
| agree_data_sharing              | Required <br /> Boolean                                                                                                                             | Does customer agree with data sharing ?                    | 
| agreements.terms_of_service     | Required <br /> Integer [1 - Yes, 0 - No]                                                                                                           | Does customer agree with terms of service?                 |
| agreements.data_proccessing_policy | Required <br /> Integer [1 - Yes, 0 - No]                                                                                                        | Does customer agree with data processingpolicy             |         



# Countries

| Country                         |
|---------------------------------|
| Spain                           |
| Germany                         |
| Argentina                       |
| Bolivia                         |
| Brazil                          |
| Bulgaria                        |
| Russia                          |
| Chile                           |
| China                           | 
| Colombia                        |
| Ecuador                         |
| Philippines                     |
| France                          |
| Italy                           |
| India                           |
| Morocco                         |
| Paraguay                        |
| Peru                            |
| Portugal                        |
| Poland                          |
| Other                           |



# Regular expressions
first_name - `/^[a-záéèíñóúüç[:space:]-\']+$/ui`

last_name  - `/^[a-záéèíñóúüç[:space:]-\']+$/ui`

second_last_name - `/^[a-záéèíñóúüç[:space:]-\']+$/ui`

bank_account - `/(ES|PT).*/`

address - `/^[a-záéèíñóúüç[:space:]-\']+$/ui`

address[city] - `/^[a-záéèíñóúüç[:space:]-\']+$/ui`

address[house_number] - `/^\d+[a-záéèíñóúüç]?$/ui`

address[postal_code] - `/^(0[1-9]|[1-4][0-9]|5[0-2])[\-]{0,1}\d{3}$/`

secondary_address[house_number] - `/^\d+[a-záéèíñóúüç]?$/ui`

secondary_address[postal_code] - `/^(0[1-9]|[1-4][0-9]|5[0-2])[\-]{0,1}\d{3}$/`



### JSON examples for /v3/customers

**POST /v3/customers**

| Headers      |                  |
|--------------|------------------|
| Content-Type | application/json |
| Accept       | application/json |

Body:
```json
{  
   "first_name":"Rodrigo",
   "last_name":"Quezada",
   "second_last_name":"Quezada",
   "nationality":"Spain",
   "phone":"711830551",
   "phone_plan":"prepaid",
   "email":"jaime.vigil@mailinator.com",
   "personal_id":"74180810Z",
   "bank_account":"ES2985991120311146424821",
   "occupation":"EMPLOYED_INDEFINITE_PERIOD",
   "housing_type":"RENTED_ROOM",
   "marital_status":"MARRIED",
   "education":"MASTER",
   "address": {
      "city":"Poveda de las Cintas",
      "street":"Rúa Olmos",
      "house_number":"94",
      "postal_code":"23013"
   },
   "lives_at_registered_address":"1",
   "secondary_address": {
      "city":"Poveda de las Cintas",
      "street":"Rúa Olmos",
      "house_number":"94",
      "postal_code":"23013"
   },
   "neto_income":"2000",
   "monthly_expenses":"600",
   "remuneration_deadline":"2017-11-19",
   "bad_credit_history":"0",
   "dependant_count":"1",
   "signature":{  
      "api_key":"spain",
      "timestamp":1510256918,
      "hash":"1f9158037d701039236cedc1b6efcd74bd0dc1cb"
   },
   "agree_electronic_services":"1",
   "agree_personal_data_protection":"1",
   "agree_data_sharing":"1",
   "agreements.terms_of_service":"1",
   "agreements.data_proccessing_policy":"1"
}
```

**Successful response for /v3/customers**

Status code: 2xx

Response:
```json
{
   "uuid": "c35c40b7-7fba-4dad-bc40-e81652c46c7c",
   "first_name":"Rodrigo",
   "last_name":"Quezada",
   "second_last_name":"Quezada",
   "nationality":"Spain",
   "phone":"711830551",
   "phone_plan":"prepaid",
   "email":"jaime.vigil@mailinator.com",
   "personal_id":"74180810Z",
   "bank_account":"ES2985991120311146424821",
   "occupation":"EMPLOYED_INDEFINITE_PERIOD",
   "housing_type":"RENTED_ROOM",
   "marital_status":"MARRIED",
   "education":"MASTER",
   "address": {
      "city":"Poveda de las Cintas",
      "street":"Rúa Olmos",
      "house_number":"94",
      "postal_code":"23013"
   },
   "lives_at_registered_address":"1",
   "secondary_address": {
      "city":"Poveda de las Cintas",
      "street":"Rúa Olmos",
      "house_number":"94",
      "postal_code":"23013"
   },
   "neto_income":"2000",
   "monthly_expenses":"600",
   "remuneration_deadline":"2017-11-19",
   "bad_credit_history":"0",
   "dependant_count":"1"
}
```

**Failed response for /v3/customers**

Status code: 422

Response:
```json
[
   {
      "field":"email",
      "message":"Email is not valid."
   },
   {
      "field":"address.street",
      "message":"Street is not valid."
   }
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
<?xml version="1.0" encoding="UTF-8" ?>
<request>
    <first_name>Rodrigo</first_name>
    <last_name>Quezada</last_name>
    <second_last_name>Quezada</second_last_name>
    <nationality>Spain</nationality>
    <phone>711830551</phone>
    <phone_plan>prepaid</phone_plan>
    <email>jaime.vigil@mailinator.com</email>
    <personal_id>74180810Z</personal_id>
    <bank_account>ES2985991120311146424821</bank_account>
    <occupation>EMPLOYED_INDEFINITE_PERIOD</occupation>
    <housing_type>RENTED_ROOM</housing_type>
    <marital_status>MARRIED</marital_status>
    <education>MASTER</education>
    <address>
        <street>Rúa Olmos</street>
        <city>Poveda de las Cintas</city>
        <house_number>94</house_number>
        <postal_code>23013</postal_code>
    </address>
    <lives_at_registered_address>1</lives_at_registered_address>
    <secondary_address>
        <street>Rúa Olmos</street>
        <city>Poveda de las Cintas</city>
        <house_number>94</house_number>
        <postal_code>23013</postal_code>
    </secondary_address>
    <neto_income>2000</neto_income>
    <monthly_expenses>600</monthly_expenses>
    <remuneration_deadline>2017-11-19</remuneration_deadline>
    <bad_credit_history>0</bad_credit_history>
    <dependant_count>1</dependant_count>
    <signature>
        <api_key>spain</api_key>
        <timestamp>1510256918</timestamp>
        <hash>1f9158037d701039236cedc1b6efcd74bd0dc1cb</hash>
    </signature>
    <agree_electronic_services>1</agree_electronic_services>
    <agree_personal_data_protection>1</agree_personal_data_protection>
    <agree_data_sharing>1</agree_data_sharing>
    <agreements.terms_of_service>1</agreements.terms_of_service>
    <agreements.data_proccessing_policy>1</agreements.data_proccessing_policy>
</request>
```

**Successful response for /v3/customers**

Status code: 2xx

Response:
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<response>
    <uuid>c35c40b7-7fba-4dad-bc40-e81652c46c7c</uuid>
    <first_name>Rodrigo</first_name>
    <last_name>Quezada</last_name>
    <second_last_name>Quezada</second_last_name>
    <nationality>Spain</nationality>
    <phone>711830551</phone>
    <phone_plan>prepaid</phone_plan>
    <email>jaime.vigil@mailinator.com</email>
    <personal_id>74180810Z</personal_id>
    <bank_account>ES2985991120311146424821</bank_account>
    <occupation>EMPLOYED_INDEFINITE_PERIOD</occupation>
    <housing_type>RENTED_ROOM</housing_type>
    <marital_status>MARRIED</marital_status>
    <education>MASTER</education>
    <address>
        <street>Rúa Olmos</street>
        <city>Poveda de las Cintas</city>
        <house_number>94</house_number>
        <postal_code>23013</postal_code>
    </address>
    <lives_at_registered_address>1</lives_at_registered_address>
    <secondary_address>
        <street>Rúa Olmos</street>
        <city>Poveda de las Cintas</city>
        <house_number>94</house_number>
        <postal_code>23013</postal_code>
    </secondary_address>
    <neto_income>2000</neto_income>
    <monthly_expenses>600</monthly_expenses>
    <remuneration_deadline>2017-11-19</remuneration_deadline>
    <bad_credit_history>0</bad_credit_history>
    <dependant_count>1</dependant_count>
</response>
```

**Failed response for /v3/customers**

Status code: 422

Response:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<response>
    <item>
        <field>personal_id</field>
        <message>Personal ID cannot be blank.</message>
    </item>
    <item>
        <field>address.postal_code</field>
        <message>Postal code cannot be blank.</message>
    </item>
</response>
```

### POST /v3/leads?customer={customer_uuid} *

Creates lead for existing customer.

* Replace customer_uuid placeholder with received uuid value from POST /v3/customers response.

| Field                | Validations                                | Description                                             |
|----------------------|--------------------------------------------|---------------------------------------------------------|
| loan_sum             | Required <br /> Must be valid number       | Amount of credit requested by customer                  |
| loan_period          | Required <br /> Must be valid number       | Term for credit requested                               |
| ip_address           | Must be valid IP address                   | IP address of customer                                  |
| aff_lead_id          | Optional <br /> free text                  | Your lead ID                                            |
| signature            | Required <br /> object <br /> contains signature fields| request signature                                       |
| signature[timestamp] | Required                                   | unix timestamp, must be UTC +/- 60 seconds              |
| signature[api_key]   | Required                                   | api key                                                 |
| signature[hash]      | Required                                   | sha1 concatenation of timestamp, api key and secret key |


### JSON examples for /v3/leads

**POST /v3/leads?customer=c35c40b7-7fba-4dad-bc40-e81652c46c7c**

| Headers      |                  |
|--------------|------------------|
| Content-Type | application/json |
| Accept       | application/json |


Body:

```json
{
    "loan_sum": "50",
    "loan_period": "1",
    "ip_address": "127.0.0.1",
    "signature": {
        "api_key": "6acc49b6-540c-512f-b1b8-7e1a9eb7d7a5",
        "timestamp": 1465310979,
        "hash": "051d1de34995444d44ab344e62e5775727f10f36"
    }
}
```

**Successful response for /v3/leads**

Status code: 2xx

Response: 
```json
{
	"loan_sum": "250.00",
	"loan_period": "30",
	"uuid": "76171735-1c22-5564-8438-fb597523fdeb",
	"status": "UNDER_INVESTIGATION",
	"date_created": "2017-01-04 08:33:09",
	"product": "PAYDAY"
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


**POST /v3/leads?customer=c35c40b7-7fba-4dad-bc40-e81652c46c7c**

Body: 
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<request>
	<loan_sum>50</loan_sum>
	<loan_period>1</loan_period>
	<ip_address>127.0.0.1</ip_address>
	<signature>
		<api_key>6acc49b6-540c-512f-b1b8-7e1a9eb7d7a5</api_key>
		<timestamp>1465310979</timestamp>
		<hash>051d1de34995444d44ab344e62e5775727f10f36</hash>
	</signature>
</request>
```

**Successful response for /v3/leads?customer=318d4a7b-b39e-58f2-a413-caefbe1615b7**

Status code: 2xx

Response:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<response>
    <loan_sum>50.00</loan_sum>
    <loan_period>1</loan_period>
    <uuid>5badfe4e-a157-55a4-bcdb-f9f3754ea054</uuid>
    <status>UNDER_INVESTIGATION</status>
    <date_created>2017-01-04 17:25:00</date_created>
</response>
```

**Failed response for /v3/leads?customer=318d4a7b-b39e-58f2-a413-caefbe1615b7**

Status code: 422

Response:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<response>
    <item>
        <field>loan_sum</field>
        <message>Loan Sum cannot be blank.</message>
    </item>
</response>
```

# Addendum A - ENUMs

### marital_status

| value          | English      | Spanish                   |
|----------------|--------------|---------------------------|
| single         | Single       | Soltero/a                 |
| married        | Married      | Casado/a                  |
| divorced       | Divorced     | Divorciado/a              |
| with_partner   | With partner | Pareja de Hecho           |
| widow          | Widow        | Viudo/a                   |

### education

| value             | English           | Spanish                     |
|-------------------|-------------------|-----------------------------|
| NO_EDUCATION      | No education      | Falta                       |
| BASIC_SCHOOL      | Basic school      | Primer ciclo de secundaria  |
| HIGH_SCHOOL       | High school       | Educación secundaria        |
| BACHELOR          | Bachelor’s degree | Promedio                    |
| MASTER            | Master            | Maestría                    |
| PHD               | Ph. D             | Mayor doctorado             |
| INDUSTRIAL_SCHOOL | Industrial school | Profesional                 |

### housing_type

| value                     | English                | Spanish                        |
|---------------------------|------------------------|--------------------------------|
| RENTED_ROOM               | Rented room            | Habitación de alquiler         |
| RENTED_APARTMENT_OR_HOUSE | Rented apartment/house | Casa o Apartamento de alquiler |
| OWN_HOUSE_OR_APARTMENT    | Own apartment/house    | Casa o apartamento propio      |
| WITH_PARENTS              | Living with parents    | Domicilio familiar             |

### occupation

| value                      | English                       | Spanish                                                     |
|----------------------------|-------------------------------|-------------------------------------------------------------|
| EMPLOYED_INDEFINITE_PERIOD | Employed indefinitely         | Empleado                                                    |
| ECONOMIC_ACTIVITY          | Economic activity             | Trabajador por cuenta propia                                |
| STUDENT                    | Student                       | Estudiante                                                  |
| MATERNITY_LEAVE            | Materniry leave               | Ama de casa                                                 |
| UNEMPLOYED                 | Unemployed                    | Desempleado                                                 |
| PENSIONER1                 | Pensioner                     | Jubilado/Pensionista                                        |

# Versions

- 1.0 (2018-07-17): first publish
