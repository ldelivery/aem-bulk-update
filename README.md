# aem-bulk-update
Postman collection to Bulk-Update AEM Meta Descriptions.

**Bonus:** We will generate the new meta descriptions with ChatGPT.

## Pre-Requisites
 * Download & Install Postman -> https://www.postman.com/downloads (MacOS, Windows, Linux)
 * Import postman.json into your local Postman instance

## Step 1 - Extract pages & properties from AEM
 * We are using the Query Builder API to get the pages and properties out of AEM [1]
 * This is the first of three requests in our postman.json
 * **Input**: You need to configure the collection with the following variables: ChatGPTURL, AEMURL, AEMuser, AEMpw
 * **Output**: You get CSV output on the postman console of the pages with the following properties: path,excerpt,name,title,lastModified,created -> Save this output to a file e.g. aem-pages.csv
 
 
## Step 2 - Transform page titles into meta descriptions using ChatGPT
 * ChatGPT will create the new descriptions for us, one by one using a Postman Runner.
 * This is the second of three requests in our postman.json
 * You need to configure the request with a valid ChatGPT API Token [2]. To be configured in the "Authorization" Tab, with type "Bearer Token".
 * Open a Postman Runner (File -> New Runner Tab), Select your CSV file, Pull Step 2 ... into the runner and uncheck Step 1 & Step 3 in the "Run order" area. 
 * **Input**: CSV file from the previous request e.g. aem-pages.csv
 * **Output**: You get CSV output on the postman console of the pages with the properties: title, meta_description -> Save this output to a file e.g. aem-pages-new-meta-descriptions.csv


## Step 3 - Load new meta descriptions back into AEM
 * We send the new meta descriptions back to AEM using another Postman runner.
 * This is the third of three requests in our postman.json
 * Open a Postman Runner (File -> New Runner Tab), Select your CSV file, Pull Step 3 ... into the runner and uncheck Step 1 & Step 2 in the "Run order" area. 
 * **Input**: CSV file from the previous request. e.g. aem-pages-new-meta-descriptions.csv

## Step 4 - Final Check & Replication
 * The last thing to do is to check if the new meta descriptions have been update in the AEM Author.
 * And of course, replicate all the changed pages to the publish instances.


--
**References**
* [1] [AEM Query Builder API](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=en)
* [2] [Directlink to create API Key for ChatGPT - Account needed](https://platform.openai.com/account/api-keys)
