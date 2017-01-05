# QUICK START API REST TANAGURU V0.1

Quickstart Api Tanaguru

## RESUME

Use Api Tanaguru with a chrome extension

## Team Tanaguru

www.tanaguru.com

## INTRODUCTION 

The API REST provides a convenient, powerful and simple Web services API to interact with the Audit Service of Tanaguru web pages. It has advantages such as great ease of integration and development. It is also an excellent solution for use with mobile and web applications. 

The use of the API requires a basic knowledge of Web services. 

API base URL: https://www.api.tanaguru.com/v1.0/service

All API calls begin with this URL with the path to the desired action. 

Sample : https://www.api.tanaguru.com/v1.0/service/auditPage 

The API is accessible only with the Https protocol and accepts HTTP **POST and GET requests**. Data sent in POST must conform to the **JSON** format.

## AUTHENTIFICATION 

The REST API uses the OAuth protocol to allow application users to access data securely without having to reveal user name and password.

Before making REST API calls, you must authenticate using OAuth 2.0. To do this, you will need the following: 

* A client ID and secret code provided by Océane consulting. 
* Make a POST security / authenticate request to exchange these authentication informations and generate an authentication token through the API. Then for all queries, you must specify in the authentication parameter on the query header the token as the value.

![Shema d'authentification, description ci-dessous](https://raw.githubusercontent.com/Tanaguru/Doc-API-REST/master/assets/capture07.png)

**Step 1 : Autentication**

1. Make a POST request on security / authenticate using the key and client code to request a token.
2. The Tanaguru API receives the request and generates a token and returns it to the client in JSON format.
3. The client recieves the token and parse it to retrieve the token value for use in step

**Step 2 : Calls to the API**

1. The client initiates a POST request on / service / auditPage with the token value as an identifier to start an audit and retrieve the results.
2. The Tanaguru API processes the query, starts the audit and returns the result data in JSON format to the client.
3. The customer receives the data and parses it for various uses.

**The tokens are passwords**

Keep in mind that the client ID, PIN, and token are required to access application-related requests. These values should be considered sensitive and just as passwords should not be shared or distributed to untrusted people. 

**SSL absolutely necessary**

This authentication is only reliable when SSL is used. Therefore, all requests to obtain and use tokens must use HTTPS parameters, it is also a requirement of the API.

### STEP 1 : ENCODING THE CLIENT'S KEY AND SECRET CODE

Steps for encoding client ID and client secret code into a set of identification informations to obtain a token:

1. Concatenate the Client ID plus the character of two points " :", and the secret code of the client, all in a single chain.
2. Encoding in Base64 the chain obtained from the previous step.

Below are examples of values indicating the result of this algorithm. Note that the client secret code used for these samples is disabled and will not work for actual requests.

Key | b9025c56b2425053dc069585390ab7c8
----|---------------------------------
Secret code | 10ee8cf96f9e35f7207a9a5cb3f89ed63c5f59692e1782d82c7eb73d21067695
Concatenate | b9025c56b2425053dc069585390ab7c8 : 10ee8cf96f9e35f7207a9a5cb3f89ed63c5f59692e1782d82c7eb73d21067695
Authentication identification informations (Base64) | YjkwMjVjNTZiMjQyNTA1M2RjMDY5NTg1MzkwYWI3Yzg6MTBlZThjZjk2ZjllMzVmNzIwN2E5YTVjYjNmODllZDYzYzVmNTk2OTJlMTc4MmQ4MmM3ZWI3M2QyMTA2NzY5NQ==

### STEP 2 : OBTAIN A TOKEN

The value calculated in step 1 must be exchanged against a token by issuing a POST / service / security / authenticate request:

* The request must be an HTTP POST request.
* The request must include an **Authorization** header with the base value : **Basic <coded value of step 1>**.
* The request must include a **Content-Type** header with the value **application / json**.
* The request must include a **Grant_type** header: **client_credentials**. 

Sample request:

    POST /v1.0/service/security/authenticate HTTP/1.1
    Host: api.tanaguru.com
    User-Agent: My App v1
    Authorization: Basic YjkwMjVjNTZiMjQyNTA1M2RjMDY5NTg1MzkwYWI3Yzg6MTBlZThjZjk2ZjllMzVmNzIwN2E5YTVjYjNmODllZDYzYzVmNTk2OTJlMTc4MmQ4MmM3ZWI3M2QyMTA2NzY5NQ==
    Content-Type: application/json
    Grant_type: client_credentials

If the request has been formatted correctly, the server responds with a coded JSON payload:

Sample of answer:

    HTTP/1.1 200 OK
    Status: 200 OK
    Content-Type: application/json; charset=utf-8
    ...
    {‘’status’’: 200 ,’’token_type’’: ‘’bearer’’ , ‘’access_token’’: ‘’AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA%2FAAAAAAAAAAAA’’}

Requests must verify that the value associated with the token_type key of the returned object is "**bearer**". The value associated with the access_token key is the token.

Note that a token is valid for one application at a time. Redoing another request with the same identification informations will return the same token until it is invalidated. 

### STEP 3 : AUTHENTIFY API REQUESTS WITH THE TOKEN

The token is used to make calls to the API. To use the token, build a normal HTTPS request and include an **Authorization** header with the value of the token: **Bearer < Token value of step 2>**. Signature is not required.

Sample request:

    GET /v1.0/service/limit_stat HTTP/1.1
    Host: api.tanaguru.com
    User-Agent: My App v1.0
    Authorization: Bearer AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA%2FAAAAAAAAAAAAAAA

## CREDITS

The Tanaguru API is based on a **credit system**.

When you run an audit from the API, and it generates a complete end result, your account is debited from a credit unit.

The other queries, (failed analysis, statistical information request, authentication, etc.) do not debit any credit.

## RETURN STATUS, LIST OF CODES

Each call to the API gives rise to a response returning a specific code according to the result obtained. Scanning this code ensures that the query has been successfully processed.

All codes >= 400 indicate that the request has not been processed successfully by our servers.

* **200**: OK
* **400**: Missing parameter, or incorrect value
* **401**: Authentication required (token not specified or invalid)
* **403**: Unauthorized action (out-of-print credits, unauthorized URL, etc.)
* **404**: Non-accessible page (URL unknown / not access the address)
* **406**: The JSON indicated in POST data is invalid
* **408**: Exceeding the maximum time allowed for audit
* **500**: Unknown Error, contact us
* **503**: The API is temporarily unavailable, try again in a few minutes

## LIST OF AVAILABLE ACTIONS

### LAUNCH AN AUDIT OF PAGE 

    **POST** https://www.api.tanaguru.com/v1.0/service/auditPage

**Request format**

    Headers 
    Authorization: bearer eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI.oGxndCq3Ocz6CKi2aUb4uA
    Accept: application/json; charset=utf-8
    Content-Type: application/json
    
    Payload
    {
    "page_url": "",
    "referentiel ": "", 
    "level ": "", 
    "language": "", 
    "dt_tbl_marker": "" ,
    "cplx_tbl_marker": "",
    "pr_tbl_marker": "", 
    "dcr_img_marker": "", 
    "inf_img_marker": "",
    "screen_width ": 1920, 
    "screen_height": 1080,
    "description_ref": true,
    "html_tags": false
    }

**Parameters**

**Name**		| **Required**	| **Type** 	| **default value**	| **Description**																| **Available value**
----------------|---------------|-----------|-------------------|-------------------------------------------------------------------------------|----------------------
Authorization	| Yes 			| String	| Any 				| Authentication token used 													| bearer <token value>
page_url 		| Yes 			| String	| Any	 			| URL of the page to be audited 												| http://www. …..
referentiel 	| No 			| String	| RGAA30 			| The accessibility guidelines to be used										| RGAA32016, RGAA30, RGAA22, AW22
level 			| No 			| String	| AA 				| The level of accessibility to be used 										| A, AA, AAA, Bz, Or, Ar, LEVEL_1, LEVEL_2, LEVEL_3
language		| No 			| String	| All				| Message language and audit result remarks										| All, Fr, En, Es_En, Es_Fr
dt_tbl_marker	| No 			| String	| Any				| HTML marker for data tables													| Corresponds to the attribute "id", "class" or "role" of complex arrays. Several markers can be entered, separated by one ";"
cplx_tbl_marker	| No 			| String	| Any				| HTML marker for complex tables												| Corresponds to the attribute "id", "class" or "role" of complex arrays. Several markers can be entered, separated by one ";"
pr_tbl_marker	| No 			| String	| Any				| HTML marker for presentation tables 											| Corresponds to the attribute "id", "class" or "role" of complex arrays. Several markers can be entered, separated by one ";"
dcr_img_marker	| No 			| String	| Any				| Decorative image marker														| Corresponds to the attribute "id", "class" or "role" of complex arrays. Several markers can be entered, separated by one ";"
inf_img_marker	| No 			| String	| Any				| Marker of images with information												| Corresponds to the attribute "id", "class" or "role" of complex arrays. Several markers can be entered, separated by one ";"
screen_width	| No 			| Number	| 1920				| Width of screen in pixel 														| Maximum value is 2048
screen_height	| No 			| Number	| 1080				| Screen height in pixels														| Maximum value is 2048
description_ref	| No 			| Boolean	| False				| To provide or not the titles of the tests, criteria, theme of the repository. | True/False
html_tags		| No 			| Boolean	| False				| To provide remarks and titles encoded in htm or not 							| True/False

**Answer format**

    {
    "http_status_code": 200,
    "url": "",
    "status": "COMPLETED",
    "score": 100,
    "ref": "",
    "level": "",
    "language": "All",
    "nb_w3c_invalidated": 0,
    "nb_passed": 0,
    "nb_failed": 0,
    "nb_not_tested": 0,
    "nb_na": 0,
    "nb_failed_occurences": 0,
    "nb_detected": 0,
    "nb_suspected": 0,
    "nb_nmi": 0,
    "themes_description_en": {
    "Rgaa30-13": "…",
     …
      },
        
    	"themes_description_fr": {
        "Rgaa30-13": "…",
         …
      },
    "themes_description_es": {
        "Rgaa30-13": "…",
         …
      },
      "criterions_description_en": {
        "Rgaa30-12-11": "…",
       …
      },
      "criterions_description_fr": {
        "Rgaa30-12-11": "…",
       …
      },
      "tests_description_en": {
        "Rgaa30-11-1-2":"…",
        …
      },
      "tests_description_fr": {
        "Rgaa30-11-1-2":"…",
        …
      }, 
     "test_na": [
        {
          "criterion": "…",
          "test": "…",
          "theme": "…"
        },
        {…}
       ],
      "test_passed": [
        {
          "criterion": "…",
          "test": "…",
          "theme": "…"
        },
        {…}
       ],
      "remarks": [
        {
          "criterion": "",      
          "theme": "",
          "test": "",
          "issue": "",
          "message_en": "",
          "message_fr": "",
          "message_es": "",
          "line_number": 0,
          "snippet": ""
        }, 
        {…….}
      ]
    }

**Object**

**Name**					| **Type**	| **Description** 
----------------------------|-----------|-------------------
http_status_code			| Number	| Return status query code
url							| String	| Audited URL 
status						| String	| Status of the audit 
score						| Number	| Audit score in %
ref							| String	| The repository used for the audit 
level						| String	| The level of accessibility used for auditing
language					| String	| Message language and audit result remarks
nb_w3c_invalidated			| Number	| Number of w3c errors
nb_passed					| Number	| Number of valided tests
nb_failed					| Number	| Number of invalided tests  
nb_not_tested				| Number	| Number of non-tested tests
nb_na						| Number	| Number of tests not applicable 
nb_failed_occurences		| Number	| Number of test cases invalidated
nb_detected					| Number	| See (Deleting this value) always zero
nb_suspected				| Number	| See (Deleting this value) always zero
nb_nmi						| Number	| Number of pre-qualified tests 
themes_description_en		| Objet		| Description of topics in English
themes_description_fr		| Objet		| Description of topics in French
themes_description_es		| Objet		| Description of topics in Spanish
criterions_description_en	| Objet		| Description of criteria in English
criterions_description_fr	| Objet		| Description of criteria in French
tests_description_en		| Objet		| Description of tests in English
tests_description_fr		| Objet		| Description of tests in French
test_na						| Array		| Vector contains all tests not applicable
test_passed					| Array		| Vector contains all validated tests 
remarks						| Array		| Vector contains all the notes of the audit
criterion					| String	| Code of the criterion (the description to be found in criterions_description_en for the English and criterions_description_fr for the French)
theme						| String	| Code of Theme (the description to look for in themes_description_en for English and themes_description_en for the French, themes_description_es for the Spanish)
test						| String	| Test code (description to be found in tests_description_en for english and tests_description_en for french)
issue						| String	| Remark code
message_en					| String	| Description of the remark in English
message_fr					| String	| Description of the remark in French
message_es					| String	| Description of the remark in Spanish
line_number					| Number	| Number of lines of the audited code
snippet						| String	| The HTML code concerned by the remark

**Sample Curl**

    curl --ssl-reqd -H " Authorization: bearer eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI.oGxndCq3Ocz6CKi2aUb4uA"  -H "Accept: application/json; charset=utf-8" -H "Content-Type: application/json" -X POST -d ' {"page_url": "http://www.oceaneconsulting.com", "language":"all", "dt_tbl_marker":"data", "cplx_tbl_marker":"complexe", "pr_tbl_marker":"presentation", "dcr_img_marker":"decorative", "inf_img_marker":"informative"}' https://www.api.tanaguru.com/v1.0/service/auditPage
	
### API USER STATISTICS

	GET   https://www.api.tanaguru.com/v1.0/service/limit_stat

**Request format**

	Headers 
	Authorization: bearer eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI.oGxndCq3Ocz6CKi2aUb4uA
	Accept: application/json; charset=utf-8
	Content-type: application/json

**Parameters**

**Name**		| **Required**	| **Type**	| **default value** | **Description**					| **Available value**
----------------|---------------|-----------|-------------------|-----------------------------------|-------------------
Authorization	| Yes			| String 	| Any				| Jeton d'authentification utilisé	| bearer <valeur de jeton>

**request format**

	{
	"status" : 200, 
	"quotas_limit" : 0, 
	"quotas_used": 0, 
	"number_call": 0,
	"number_call_error": 0
	}

**Object**

**Name**			| **Type**	| **Description** 
--------------------|-----------|-------------------
status				| Number	| Return status query code
quotas_limit		| Number	| Quota limitation (number of calls allowed)
quotas_used			| Number	| Number of calls made successfully
number_call			| Number	| Number of calls made with or without error
number_call_error	| Number	| Number of calls returned with an error

**Sample Curl**

	curl --ssl-reqd -H " Authorization: bearer eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI.oGxndCq3Ocz6CKi2aUb4uA" -H "Accept: application/json; charset=utf-8" -H "Content-Type: application/json" -X GET
	https://www.api.tanaguru.com/v1.0/service/limit_stat

### REQUEST A TOKEN (AUTHENTICATION)

	POST   https://www.api.tanaguru.com/v1.0/security/authenticate

**request format**

	Headers 
	Authorization: basic YjkwMjVjNTZiMjQyNTA1M2RjMDYMmM3ZWI3M2QyMTA2NzY5NQ==
	Content-Type: application/json
	Grant_type: client_credentials

**Parameters**

**Name**		| **Required**	| **Type**	| **default value** | **Description**					| **Available value**
----------------|---------------|-----------|-------------------|-----------------------------------|-------------------
Authorization	| Yes			| String	| Any				| Informations d'authentification	| basic <concaténation des clés et code secret de client encodé en Base64>

**request format**

	{
	"status ": 200,
	"token_type": "bearer”,
	"access_token": ""
	}

**Object**

**Name**	| **Type**	| **Description** 
------------|-----------|-------------------
status		| Number	| Return status query code
token_type	| String	| Token type
access_token| String	| Authentication token

**Sample Curl**

	curl --ssl-reqd -H " Authorization: basic YjkwMjVjNTZiMjQyNTA1M2RjMDYMmM3ZWI3M2QyMTA2NzY5NQ==" -H "Content-Type: application/json" -H "Grant_type: client_credentials" -X POST
	https://www.api.tanaguru.com/v1.0/service/security/authenticate

### INVALIDATE A TOKEN (AUTHENTICATION)

	POST   https://www.api.tanaguru.com/v1.0/security/invalidate_token

**Request format**

	Headers 
	Authorization: basic YjkwMjVjNTZiMjQyNTA1M2RjMDYMmM3ZWI3M2QyMTA2NzY5NQ==
	Content-Type: application/json
	access_token: …

**Parameters**

**Name**		| **Required**	| **Type**	| **default value** | **Description**					| **Available value**
----------------|---------------|-----------|-------------------|-----------------------------------|-------------------
Authorization	| Yes			| String	| Any				| Authentication information		| basic < Key concatenation and base64 encoded client secret code>
access_token	| Yes 			| String	| Any				| Authentication Token to Disable	| Token value

**Request format**

	{
	"status ": 200,
	"access_token": ""
	}

**Object**

**Name**		| **Type**	| **Description**
----------------|-----------|-------------------
status			| Number	| Return status query code
access_token	| String	| Authentication token that has been invalidated 

**Sample Curl**

	curl --ssl-reqd -H " Authorization: bearer eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI.oGxndCq3Ocz6CKi2aUb4uA" -H "Content-Type: application/json" -H "access_token:" -X POST
	https://www.api.tanaguru.com/v1.0/service/security/invalidate_token

## CASES OF ERRORS 

This section describes some common mistakes involved in using tokens. Be aware that not all possible error responses are covered here! 

**Obtain a token with an invalid request (leaving Grant_type: client_credentials aside)**

	{
	"message": "grant_type value is missing",
	"status": 412
	}

**Obtain a token with an invalid request (leaving aside Authorization)**

	{
	"message": "Consumer key & Consumer secret value are missing",
	"status": 412
	}

**Obtain or invalidate a token with invalid or outdated application credentials**

	{
	"message": "Unable to verify your credentials",
	"status": 403
	}

**Disable invalid or outdated token**

	{
	"message": "Invalid or expired token",
	"status": 403
	}

## QUICK START GUIDE

### INSTALLING A CHROME EXTENSION

From a chrome browser, download Advanced Rest client (https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo)

Once installed, start the extension. 

### REQUEST TOKEN

Assuming that your Beta ID is : **b9025c56b2425053dc069585390ab7c8** 

and your Beta Password is : **10ee8cf96f9e35f7207a9a5cb3f89ed63c5f59692e1782d82c7eb73d21067695**. 

**Note**: For First Beta version of Tanaguru Api, we authorize 5000 requests for free.

**Note**: The authentication informations mentioned is only for beta version, this is a sample. Contact team Tanaguru to have a valid client ID and PIN (contact@tanaguru.com) 

1. Enter the url for authentication: **https://api.tanaguru.com/v1.0/service/security/authenticate** 
2. Choose the request method POST  
3. Go to the tab **Headers form** 
4. Add the parameter **Authorization** 
5. Click to the « Edit » icone to set the parameter value **Authorization**
6. Enter your **customer ID** at User name, your **secret code** at Password, and click on select button.
7. Add the **Grant_type** parameter and give it the value **client_credentials** 
8. Add the **Content-type** parameter and give it the value **application/json**
9. After entering all these informations, click send to apply.

![Request screenshot, step above](https://raw.githubusercontent.com/Tanaguru/Doc-API-REST/master/assets/capture01.png)

You will get the following answer:

![Request answer screenshot, step below](https://raw.githubusercontent.com/Tanaguru/Doc-API-REST/master/assets/capture01.png)

1. The answer contain informations about the answer **statut** and the **token** type 
2. The token value that you will use to make audit requests. 

**Note** : Keep the token generated to use it in the authentication for audit requests.

### INVALIDATE A TOKEN

After using the token, if you want disable it for any reason,

1. Enter the url authentication: **https://api.tanaguru.com/v1.0/service/security/invalidate_token** 
2. Make a **POST** request, 
3. Go to the tab **Headers form** 
4. Add the parameter **Authorization**, 
5. Click to the « Edit » icone to set the parameter value **Authorization**
6. Enter your **Customer ID** at User name, your **secret code** at Password, and validate.
7. Add the **Accept parameter** and give it the value **application/json ; charset=utf-8** 
8. Add the **Content-type** parameter and give it the value **application/json**
9. Add the **access_token** parameter and enter the token that you want invalidate. 
10. Click send to apply.

![Request screenshot, step above](https://raw.githubusercontent.com/Tanaguru/Doc-API-REST/master/assets/capture03.png)

### LAUNCH A PAGE AUDIT

To launch an audit on the site http://www.oceaneconsulting.com with all languages,

1. Enter the url authentication: **https://api.tanaguru.com/v1.0/service/auditPage**
2. Make a **POST** request, 
3. Choose **application/json** as data type to send. 
4. Go to tab **Headers form** 
5. Add the parameter **Authorization**
6. Use the Token type **bearer** with your token as : **bearer eyJH...**
7. Add the parameter **Accept** and give it the value */*
8. Add the parameter **Content-type** and give it the value **application/json**
9. Got to tab **Raw payload**
10. Enter at the Json format {**"page_url" : "http://www.oceaneconsulting.com"}**, add the **language, referentiel** et **level** attributes is not required because they have a default value (All, Rgaa30, AA). 
11. Click send to apply.	

![Request screenshot, step above](https://raw.githubusercontent.com/Tanaguru/Doc-API-REST/master/assets/capture04.png)

### API USE STATISTICS 

To see API usage statistics with your account 

1-	Enter the url authentication: **https://api.tanaguru.com/v1.0/service/limit_stat**
2-	Make a **GET** request
3-	Go to tab **Headers form**
4-	Add the parameter **Authorization**
5-	Use the Token type **bearer** with your token as : **bearer eyJH...**
6-	Add the parameter **Accept** and give it the value */*
7-	Add the parameter **Content-type** and give it the value **application/json**
8-	Click send to apply.	
9-	The answer contain the API usage informations.  

![Request screenshot, step above](https://raw.githubusercontent.com/Tanaguru/Doc-API-REST/master/assets/capture05.png)
