# Capstone Project: Lab5-Custom-Skills.

In this lab, you will learn [how to create a Custom Skill](https://docs.microsoft.com/en-us/azure/search/cognitive-search-custom-skill-interface)
and integrate it into the enrichment pipeline. Custom skills allow you to add any REST API transformation to the dataset, also feeding other transformations within the pipeline, if required.

The Azure Content Moderator API is a cognitive service that checks **text, image, and video** content for material that is potentially offensive, risky, or otherwise undesirable.
When such material is found, the service applies appropriate labels (flags) to the content. Your app can then handle flagged content in order to comply with regulations or maintain the intended environment for users.
See the [Content Moderator APIs](https://docs.microsoft.com/en-us/azure/cognitive-services/content-moderator/overview#content-moderator-apis) section to learn more about what the different content flags indicate.

This API offers:

+ Text moderation: Scans text for offensive content, sexually explicit or suggestive content, profanity, and personally identifiable information (PII). **That's what you will use in this lab**.
+ Custom term lists: Scans text against a custom list of terms in addition to the built-in terms. Use custom lists to block or allow content according to your own content policies.
+ Image moderation: Scans images for adult or racy content, detects text in images with the Optical Character Recognition (OCR) capability, and detects faces.
+ Custom image lists: Scans images against a custom list of images. Use custom image lists to filter out instances of commonly recurring content that you don't want to classify again.
+ Video moderation: Scans videos for adult or racy content and returns time markers for said content.
+ Review: Use the Jobs, Reviews, and Workflow operations to create and automate human-in-the-loop workflows with the human review tool. The Workflow API is not yet available through the .NET SDK.

Content Moderator’s machine-assisted **text moderation** response includes the following information:

+ Profanity: term-based matching with built-in list of profane terms in various languages
+ Classification: machine-assisted classification into three categories
+ Personally Identifiable Information (PII)
+ Auto-corrected text
+ Original text

Content Moderator API also has a tool for human review of the moderation. For more information, click [here](https://docs.microsoft.com/en-us/azure/cognitive-services/content-moderator/review-tool-user-guide/human-in-the-loop
).

## Challenge
In this challenge, your team will perform the following task:
1. Create a web API custom skill for your Azure Search index. 
2. Integrate your custom skill into your Azure Search Index
3. Update your client application to enable users filter search results by specifying keywords.

## Sucess Criteria
To complete this challenge successfully, your team must:
* Implement a custom skill web service that identifies PII information in a document
* Add a field to your search index that is populated by your custom skill
* Modify your client application so that users can filter search results on the presence of a specified keyword. 

**Hints :**
You can also write your custom skills using Azure Functions in Python from VS Code and publish to Azure.

If your team prefer to do this challenge with Visual Studio, please follow through the guide that have been provided below and helper code to step through. 

## Resources 
* [Example - Custom Skills](https://docs.microsoft.com/en-us/azure/search/cognitive-search-create-custom-skill-example)
* [Custom Skills Interface](https://docs.microsoft.com/en-us/azure/search/cognitive-search-custom-skill-interface)
* [Azure Functions Documentation](https://docs.microsoft.com/en-us/azure/azure-functions/)

**Hint :**
You will use an [Azure Function](https://azure.microsoft.com/services/functions/) to wrap the [Content Moderator API](https://azure.microsoft.com/en-us/services/cognitive-services/content-moderator/) and detect incompliant documents in the dataset.

Ensure the - Content Moderator API service is running in your Azure Subscription with a service pricing tier of **S0**.

> **Note** We can't use the free tier since it only allows 1 TPS (transaction per second).

To test your new resource, navigate to the [Content Moderator Text API console](https://docs.microsoft.com/en-us/azure/cognitive-services/content-moderator/try-text-api). Read the whole page, it should take you about four minutes. When you are done, scroll all the way up and click the first link on the page, [Text Moderation API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f). It will open a control panel for Cognitive Services, as you can see in the image below.

![Cognitive Services Panel](../resources/images/lab-custom-skills/panel.png)

Now, clicking the blue buttons, choose the region where you created your Content Moderator API, that should be the same region you are using for all services in this training. Now set the values as listed below:

+ Query parameters
  + autocorrect = true
  + PII = true
  + listId = remove parameter (we don't have a list of prohibited terms to work with at this point)
  + classify = true
  + language = eng (The default example is in English)

+ Headers
  + Keep Content-Type as "text/plain"
  + Paste in your Content Moderator API key

Scroll down and check the suggested text in "Request body" section. It has PIIs like email, phone number, physical and IP addresses. It also has a profanity word. Scroll until the end of the page and click the blue "Send" button. The expected results are:

+ Response Status = 200 OK
+ Response Latency <= 1000 ms
+ Response content with a json file where you can read all of the detected problems.

> **Note** The "Index" field indicates the position of the term within the submitted text.

## IDE-Visual Studio

Visual Studio 2019 has [Tools for AI](https://visualstudio.microsoft.com/downloads/ai-tools-vs/) but you don't need it for this training since they are used to **create** custom AI projects, while in this training you are **using** AI through Azure Cognitive Search and Services.

### Preparing the solution

1. Open the Windows Explorer and find the folder **resources/code-azure-function** where you cloned this repository. You need to locate the file CognitiveSkill.sln. There should be a CognitiveSkill folder in the same folder where you found the .sln file.

2. Double click, or hit enter, on the .sln file to open it in Visual Studio 2019. Check your Solution Explorer on the right and confirm that you can see the same structure of the image below.

![Solution Structure](../resources/images/lab-custom-skills/structure.png)

In the image below you can see very important aspects of the code you are about to edit:

+ In the first red square you can see the code testing the return of the Content Moderator API for Personal Identifiable Information (PII). The code will return if the documents of the dataset have PII or not.
+ The second red area highlights the "text" label. You will need to use this label in the skillset definition of this lab, in one of the next steps.
+ The other 3 read areas indicate where you need to edit the code, what will be explained right after the image, within this step of the lab.

![C# code](../resources/images/lab-custom-skills/code.png)

>**Note** This Azure Function application is a way to add the custom skill to the enrichment pipeline. Please note that good practices are not 100% used in the code (e.g. the key wide open and fixed in the code). For enterprise grade solutions, this code should be adapted to all good practices, business and security requirements.

1. Click on the **Moderation.cs** file on the Solution Explorer, it should open in the main window of your Visual Studio. Get familiar with the "using session", to learn which packages are used. Scroll down until the three "TO DO - Action Required" sessions of the code.

1. Follow the instructions of the three "TO-DO" sections:

+ If necessary change "southcentralus" in the uriPrefix URL. This is the same endpoint of the first step of this lab
+ Add your key. This is the same key of the first step of this lab
+ If necessary, change the host url with the same endpoint of the first step of this lab, but without `https://` and without `/contentmoderator`

>**Note** Our dataset has a text file with PII, in english. That's why we are only testing this capability, to enforce this regulation within the business documents. This simple usage was defined to keep the training simple. For advanced scenarios, you can use all other Content Moderator capabilities.
>**Note**: This Azure Function output is **Edm.String**, but this lab converts it to a boolen information: if the document has moderated content or not.

## Step 3 - Test the function from Visual Studio

1. Press **F5** or click the green arrow on the "ContentModerator" button to run the solution.

1. If prompted, click **Allow access**

1. You should expect a "cmd" window with some information and the local URL of the endpoint in the end of the log, like you can see in the image below.

![Cmd window](../resources/images/lab-custom-skills/cmd.png)

1. Now use Postman, without any information in headers tab, to issue a call like the one shown below:

```http
POST http://localhost:7071/api/ContentModerator
Content-Type: application/json
```

### Request body

```json
{
   "values": [
        {
            "recordId": "a1",
            "data":
            {
               "text":  "Email: abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052"
            }
        }
   ]
}
```

### Response

You should see a response similar to the following example:

```json
{
    "Values": [
        {
            "RecordId": "a1",
            "Data": {
                "text": true
            },
            "Errors": [],
            "Warnings": []
        }
    ]
}
```

1. Close down the Azure Function CLI window to stop debugging.

## Step 4 - Publish the function to Azure

When you are satisfied with the function behavior, you can publish it.

1. In **Solution Explorer**, right-click the project and select **Publish**. Choose **Create New** > **Publish**.

2. If you haven't already connected Visual Studio to your Azure account, select **Add an account....**

3. Follow the on-screen prompts. You are asked to specify the Azure account, the resource group, the hosting plan, and the storage account you want to use. You will have the option to create a new resource group (or use the one you've already created for these labs), a new hosting plan, and a storage account if you don't already have one in your Azure Account. When finished, select **Create**.

4. After the deployment is complete, copy the Site URL to notepad or similar. It is the address of your function app in Azure. You can always get this URL from the Azure Portal using [this](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-first-azure-function#test-the-function) simple steps.

5. In the [Azure portal](https://portal.azure.com), navigate to the Resource Group, and look for the Content Moderator Function you published. Under the **Manage** section, you should see Host Keys. Select the **Copy** icon for the *default* host key and copy to notepad or similar.

## Step 5 - Test the function in Azure

Now that you have the default host key, use Postman to test your function as follows:

```http
POST https://[your-function-url].azurewebsites.net/api/ContentModerator?code=[enter your Azure Functions key here]
```

### Request Body

```json
{
   "values": [
        {
            "recordId": "a1",
            "data":
            {
               "text":  "Email: abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052"
            }
        }
   ]
}
```

This should produce a similar result to the one you saw previously when running the function in the local environment.

## Step 6 - Cleaning the environment again

Let's do the same cleaning process of the previous lab. Save all scripts (API calls) you did until here, including the definition json files you used in the "body" field.

You can use Azure Portal or API calls to do the required deletes:

1. [Deleting the indexer](https://docs.microsoft.com/en-us/rest/api/searchservice/delete-indexer) - Just use your service, key and indexer name
1. [Deleting the index](https://docs.microsoft.com/en-us/rest/api/searchservice/delete-index) - Just use your service, key and indexer name
1. [Deleting the Skillset](https://docs.microsoft.com/en-us/rest/api/searchservice/delete-skillset) - Just use your service, key and skillset name

Status code 204 is returned on a successful deletion.

## Step 7 - Connect to your Pipeline, recreating the environment

Now let's use the [official documentation](https://docs.microsoft.com/en-us/azure/search/cognitive-search-custom-skill-interface) to learn the syntax we need to add the custom skill to our enrichment pipeline.

As you can see, the custom skill works like all other predefined skills, but the type is **WebApiSkill** and you need to specify the URL you created above. The example below shows you how to call the skill. Because the skill doesn't handle batches,
you have to add an instruction for maximum batch size to be just ```1``` to send documents one at a time. The objectives are:

1. Save the extracted boolean value for moderation in a new index field

1. Also submit the OCR text to the content moderator API

1. Keep the OCR text been submited to Entity, Language and Key Phrases extractions

```json
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "Our new content moderator custom skill",
        "uri": "https://[your-function-url].azurewebsites.net/api/ContentModerator?code=[enter default host key here]",
        "batchSize":1,
        "context": "/document",
        "inputs": [
          {
            "name": "text",
            "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "text",
            "targetName": "needsModeration"
          }
        ]
      }

```

### Step 7.1 - Challenge

As you can see, again we are not giving you the body request. One more time you can use the previous lab as a reference.  

Skipping the services and the data source creation, repeat the other steps of the previous labs, in the same order. Name the new field of the content moderation process as **needsModeration**. It is case sensitive, so it is important to use the same name formation. If you decide to use a different name, you will need to change the Bot code to make it work.

>**Note** Please make sure you will create the new index field for the moderation analysis as boolean. More information [here](https://docs.microsoft.com/en-us/azure/search/search-what-is-an-index).

1. Recreate the Skillset
2. Recreate the Index
3. Recreate the Indexer
4. Check Indexer Status - Check the moderation results.  
5. Check the Index Fields - Check the moderated text new field.
6. Check the data - If you don't see the moderated data, something went wrong. Select only the needsModeration and the blob_uri fields

## Step 8

Now we have our data enriched with pre-defined and custom skills. Use the Search Explorer within your Azure Search service in the Azure Portal to query the data. Create 2 or more queries to identify documents with compliance issues, the moderated documents.

![Checking for Moderation problems](../resources/images/lab-custom-skills/moderatedtex-query-portal.png)

## Step 9 - Optional

Here is another challenge for you. Use [this](https://docs.microsoft.com/en-us/azure/search/cognitive-search-create-custom-skill-example) documentation as a reference and:

1. Create and publish another Azure Function, for the Translation API

2. Connect this new API into your Enrichment Pipeline skillset

3. Create a new skill field for the translated text into the index

4. Create another mapping in your indexer

5. Use Postman or Azure Search Explorer to check the translated data.

## Next Step

[Business Documents Bot Lab](../labs/lab6-bot-business-documents.md) or
[Back to Read Me](../README.md)
