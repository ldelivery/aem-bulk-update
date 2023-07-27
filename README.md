# AEM Bulk-Update Postman Collection

This guide describes a classic ETL (Extract, Transform, Load) process for Bulk-Updating Adobe Experience Manager (AEM) properties.

The process:
- has four steps.
- is codified into a Postman collection.
- makes use of the Postman Runner feature.
- bulk updates AEM meta description page properties as an example.
- can be adapted to update any AEM property based on your requirements.
- uses the ChatGPT language model to transform page titles into meta descriptions.

## Prerequisites

1. Download and install [Postman](https://www.postman.com/downloads) on your local machine. It's compatible with MacOS, Windows, and Linux.
2. Import the `postman.json` file into your Postman application.

## Postman Runner Setup

For each step, you'll be using the Postman Runner. To do this, go to `File -> New Runner Tab`, select your respective CSV file (it changes per step), pull the corresponding step into the runner and ensure only that step is checked in the "Run order" area.

## Step 1: Extract Pages & Properties from AEM

This step utilizes the Query Builder API [1] to extract pages and properties from AEM.

- **Input:** Configure the collection with these variables: `ChatGPTURL`, `AEMURL`, `AEMuser`, `AEMpw`.
- **Output:** Postman will display a CSV output of the pages with properties including `path`, `excerpt`, `name`, `title`, `lastModified`, and `created`. Save this output to a local file, for example, `aem-pages.csv`.

## Step 2: Transform Page Titles into Meta Descriptions Using ChatGPT

This step uses the ChatGPT API [2] to generate new meta descriptions for each page. Our example focuses on meta descriptions, keep in mind that any property could be updated in a similar manner.

- **Setup:** Configure the request with a valid ChatGPT API Token in the "Authorization" tab, with "Bearer Token" as the type.
- **Input:** CSV file from the previous step (`aem-pages.csv`).
- **Output:** A CSV output containing pages with `title` and `meta_description` properties will be shown in the Postman console. Save this output to a file, e.g., `aem-pages-new-meta-descriptions.csv`.

## Step 3: Load New Meta Descriptions Back into AEM

This step transfers the new meta descriptions back into AEM via the Sling Post Servlet [3].

- **Input:** CSV file from the previous step, i.e., `aem-pages-new-meta-descriptions.csv`.

## Step 4: Final Check and Replication

Finally, verify that the new meta descriptions have been updated in the AEM author interface. Subsequently, replicate all the updated pages to the publishing instances.

--

**References**
* [1] [AEM Query Builder API](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=en)
* [2] [Directlink to create API Key for ChatGPT - Account needed](https://platform.openai.com/account/api-keys)
* [3] [Sling Post Servlet](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html)
