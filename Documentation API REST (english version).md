# QUICK START API REST TANAGURU V0.1

Quickstart Api Tanaguru

## RESUME

Use Api Tanaguru with a chrome extension

## Team Tanaguru

www.tanaguru.com

## QUICK START GUIDE

### INSTALLING A CHROME EXTENSION

From a chrome browser, download Advanced Rest client (https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo)

Once installed, start the extension. 

### REQUEST TOKEN

Assuming that your Beta ID is : **b9025c56b2425053dc069585390ab7c8** 

and your Beta Password is : **10ee8cf96f9e35f7207a9a5cb3f89ed63c5f59692e1782d82c7eb73d21067695**. 

**Note**: For First Beta version of Tanaguru Api, we authorize 5000 requests for free.

**Note**: The authentication informations mentioned is not only for beta version, this is a sample. Contact team Tanaguru to have a valid client ID and PIN (contact@tanaguru.com) 

1. Enter the url for authentication: **https://api.tanaguru.com/v1.0/service/security/authenticate ** 
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

1. Enter the url authentication: **https://api.tanaguru.com/v1.0/service/security/invalidate_token ** 
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

1. Enter the url authentication: **https://api.tanaguru.com/v1.0/service/auditPage **
2. Make a **POST** request, 
3. Choose **application/json** as data type to send. 
4. Go to tab **Headers form** 
5. Add the parameter **Authorization**
6. Use the Token type **bearer** with your token as : **bearer eyJH... **
7. Add the parameter **Accept** and give it the value */*
8. Add the parameter **Content-type** and give it the value **application/json**
9. Got to tab **Raw payload **
10. Enter at the Json format {**"page_url" : "http://www.oceaneconsulting.com"}**, add the **language, referentiel** et **level** attributes is not required because they have a default value (All, Rgaa30, AA). 
11. Click send to apply.	

![Request screenshot, step above](https://raw.githubusercontent.com/Tanaguru/Doc-API-REST/master/assets/capture04.png)

### API USE STATISTICS 

To see API usage statistics with your account 

1-	Enter the url authentication: **https://api.tanaguru.com/v1.0/service/limit_stat **
2-	Make a **GET** request
3-	Go to tab **Headers form **
4-	Add the parameter **Authorization**
5-	Use the Token type **bearer** with your token as : **bearer eyJH... **
6-	Add the parameter **Accept** and give it the value */*
7-	Add the parameter **Content-type** and give it the value **application/json**
8-	Click send to apply.	
9-	The answer contain the API usage informations.  

![Request screenshot, step above](https://raw.githubusercontent.com/Tanaguru/Doc-API-REST/master/assets/capture05.png)