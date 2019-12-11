# Capstone Project: Lab3-Text-Skills.

# Create a Cognitive Search Enrichment Process with Text Skills
In this lab, you will learn the mechanics of programming data enrichment in Azure Search using cognitive skills. Cognitive skills are natural language processing (NLP) and image analysis operations that extract text and text representations of an image, detect language, entities, key phrases, and more. The end result is rich additional content in an Azure Search index, created by a cognitive search indexing pipeline.

All the links in this lab are extra content for your learning, but you don't need them to perform the required activities.

In this lab, you will learn how to create a Cognitive Search indexing pipeline that enriches source data in route to an index. The output is a full text searchable index on Azure Search. The image below shows you the 4 objects you will create using API CALLs.

* Plan of execution ![alt text](https://github.com/frdogbey/AI-Powered-Knowledge-Management-Capstone-Project-1/blob/master/resources/images/lab-text-skills/plan.png)


## Challenge 
The list of activities you will do, using Azure Search REST APIs or via code, is:
1.	Create a data source for the uploaded data.
2.	Create a Cognitive Search Skillset with entity recognition, language detection, text manipulation and key phrase extraction.
3.	Create an index to store the enriched metadata.
4.	Create an indexer process to execute the enrichment.
5.	Check the indexer status
6.	Check the enriched metadata
7.	Query the metadata
8.  Modify the indexing pipeline for the site documents to generate the knowledge store assets
    * The index you've created at earlier includes insights that you've extracted from the documents through    built-in and custom AI skills. These insights can add value not only to customer searches, but also to    internal data analysis that your employees routinely perform to help them identify trends and take        actionable steps.

Azure Search includes the ability to create [knowledge stores](https://docs.microsoft.com/en-us/azure/search/knowledge-store-concept-intro): persisted data structures that are populated by an indexer. 

Tip You can enhance the index with other Azure Search standard capabilities, such as [synonyms](https://docs.microsoft.com/en-us/azure/search/search-synonyms), [scoring profiles](https://docs.microsoft.com/en-us/azure/search/index-add-scoring-profiles), [analyzers](https://docs.microsoft.com/en-us/azure/search/index-add-custom-analyzers), and [filters](https://docs.microsoft.com/en-us/azure/search/search-filters).

## Hints
* You can only have one skillset per indexer
* A skillset must have at least one skill
* You can create multiple skills of the same type (for example, variants of an image analysis skill) but each   skill can only be used once within the same skillset
* Note that Knowledge store is supported in the **2019-05-06-Preview** API and later.
* Knowledge store structures are based on projections, which map fields to a JSON structure that represents     an object (a blob in Azure storage) or one or more tables (tables in Azure storage)
* You might find it easier to define projections by using a shaper skill. This enables you to control the       structure of the JSON representation of the fields you want to persist in the knowledge store.
* The first table you define should represent the base level of your field structure (which represents an       indexed document) - it will contain all fields at that level that are not explicitly specified as inputs      for other table projections.
* For each table projection, you can generate a unique key. The key generated for the base-level table will     automatically be added to other tables containing related fields.
* Azure Storage tables are automatically assigned key columns for partition and row, as well as a timestamp     column.

## Success Criteria

To complete this challenge successfully, you must:

* Modify the skillset for your index to generate the required knowledge store assets.
* Browse the blobs and tables in your knowledge store using the Storage Explorer interface for your Azure       storage account in the Azure portal.

## Challenge - Create a data source
Now that your services and source files are prepared, you can start assembling the components of your indexing pipeline. We'll begin by creating a [data source object](https://docs.microsoft.com/en-us/rest/api/searchservice/create-data-source) that tells Azure Search how to retrieve external source data.

**Note** In this step, you will provide detailed steps on where you should define settings in the Postman application so that you can become familiar with it. Later steps will not provide as detailed steps, just parameter information that you will fill into the Postman application. We recommend that you create a new Collection (Folder) for each lab and a new Request (API CALL command) for each step. And save your work so that you can reuse your commands. You can use "save as" command to reuse one Request from one lab into another lab. This will save you lots of time. Just be careful to don't overwrite previous work.

You may use Postman to call Azure Search service APIs or write code to do so. 
Note Double check you have used the all connection string, it is very long and sometimes you may have not copied it all.

## Challenge - Create a skillset

In this step, you define a set of enrichment steps that you want to apply to your data. Each enrichment step is called a skill, and the set of enrichment steps a skillset. This tutorial uses the following [predefined cognitive skills](https://docs.microsoft.com/en-us/azure/search/cognitive-search-predefined-skills):
* [Language Detection](https://docs.microsoft.com/en-us/azure/search/cognitive-search-skill-language-detection) to identify the content's language. In December 2018 only English and Spanish are supported for all cognitive skills. For the actual Cognitive Search language list and status, click [here](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/language-support). That's why the provided dataset has documents in these two languages. The Azure Search supported language list is something different, a different universe of options. For more information, click [here](https://docs.microsoft.com/en-us/azure/search/index-add-language-analyzers?redirectedfrom=MSDN).
* [Text Split](https://docs.microsoft.com/en-us/azure/search/cognitive-search-skill-textsplit) to break large   content into smaller chunks before calling the key phrase extraction skill. Key phrase extraction accepts     inputs of 50,000 characters or less. In December 2018, entity recognition is accepting only 4000, it will     be increased to 50,000 characters in the near future. But to make it work with both actual limits, this       training labs will use 4000 characters. It is required, the dataset has files much bigger than that.
* [Named Entity Recognition](https://docs.microsoft.com/en-us/azure/search/cognitive-search-skill-named-entity-recognition) for extracting the names of organizations from content in       the blob container.
* [Key Phrase Extraction](https://docs.microsoft.com/en-us/azure/search/cognitive-search-skill-keyphrases) to   pull out the top key phrases.

## Create a skillset - Sample Request
You may use code or use Postman to issue a PUT method that will create a skillset named demoskillset. 
Reference the skillset name demoskillset for the rest of this lab.

**Note** Entity Recognition skill initial types were "Person", "Location" and "Oragnization". Types "Quantity", "Datetime", "URL" and "Email" were added on November 28th, 2018. This means that Entity Recognition Skill can be used 7 times in the same skillset. But you can't use the same type twice in the same skillset.

## About the request
Let's take some time to review the request and confirm we understand what's happening and how.

Notice how the key phrase extraction skill is applied for each page. By setting the context to "document/pages/*" you run this enricher for each member of the document/pages array (for each page in the document).

Each skill executes on the content of the document. During processing, Azure Search cracks each document to read content from different file formats. Found text originating in the source file is placed into a generated content field, one for each document. As such, set the input as "/document/content".

A graphical representation of the skillset you created is shown below.
* Skillset ![alt text](https://github.com/frdogbey/AI-Powered-Knowledge-Management-Capstone-Project-1/blob/master/resources/images/lab-text-skills/skillset.png)

Outputs can be mapped to an index, used as input to a downstream skill, or both, as is the case with language code. In the index, a language code is useful for filtering. As an input, language code is used by text analysis skills to inform the linguistic rules around word breaking.

For more information about skillset fundamentals, read [how to define a skillset](https://docs.microsoft.com/en-us/rest/api/searchservice/create-skillset).

## Challenge - Create an index
In this section, you define the index schema by specifying which fields to include in the searchable index, and the search attributes for each field. Fields have a type and can take attributes that determine how the field is used (searchable, sortable, and so forth). Field names in an index are not required to identically match the field names in the source. In a later step, you add field mappings in an indexer to connect source-destination fields. For this step, define the index using field naming conventions pertinent to your search application.

This exercise uses the following fields and field types:

{| class="wikitable"
|filed-names:||id||content||languageCode||keyPhrases|| organizations
|-
|field-types:||Edm.String||Edm.String||Edm.String||List<Edm.String>||List<Edm.String>
|}

## Index-Sample Request
Before you make this REST call, remember to replace the service name and the admin key in the request below if your tool does not preserve the request header between calls.
This request creates an index. Use the index name demoindex for the rest of this tutorial.

Check the Azure portal to confirm the index was created in Azure Search. On the Search service dashboard page,verify the Indexes tile has a 2 items. You might need to wait a few minutes for the portal page to refresh. Click on Indexes to confirm that the demoindex appears.

Review the request and confirm understanding. If you want to learn more about defining an index, see [Create Index (Azure Search REST API)](https://docs.microsoft.com/en-us/rest/api/searchservice/create-index).

## Create an indexer, map fields, and execute transformations
So far you have created a data source, a skillset, and an index. These three components become part of an [indexer](https://docs.microsoft.com/en-us/azure/search/search-indexer-overview) that pulls each piece together into a single multi-phased operation. To tie these together in an indexer, you must define field mappings. Field mappings are part of the indexer definition and execute the transformations when you submit the request.

For non-enriched indexing, the indexer definition provides an optional _fieldMappings_ section if field names or data types do not precisely match, or if you want to use a function.

For cognitive search workloads having an enrichment pipeline, an indexer requires _outputFieldMappings_. These mappings are used when an internal process (the enrichment pipeline) is the source of field values. Behaviors unique to _outputFieldMappings_ include the ability to handle complex types created as part of enrichment (through the shaper skill). Also, there may be many elements per document (for instance, multiple organizations in a document). The _outputFieldMappings_ construct can direct the system to "flatten" collections of elements into a single record.

**The next step takes up to 10 minutes of processing to complete.**

## Challenge - Create an Indexer-Sample Request
Before you make this REST call, remember to replace the service name and the admin key in the request below if your tool does not preserve the request header between calls.

Expect this step to take a minute or two to complete. Even though the data set is small, analytical skills are computation-intensive. Some skills, such as image analysis, are long-running.

Check the Azure portal to confirm the index was created in Azure Search. On the Search service dashboard page,verify if the Indexers tile has 2 indexes (one from your previous lab). You might need to wait a few minutes for the portal page to refresh. Click on Indexers to confirm that the demoindexer appears.

While it is running, check this detail: The "blob_uri" was defined as the second field of the index. But it is the third mapping in the indexer. It is a good example on how they work independent. You should scroll up to see these two body requests and compare them.

**Tip** Creating an indexer invokes the pipeline. If there are problems reaching the data, mapping inputs and outputs, or order of operations, they appear at this stage. To re-run the pipeline with code or script changes, you might need to drop objects first. For more information, see Reset and re-run.

## Challenge - Exploring the request body
Your script or code should have set "maxFailedItems" to -1, which instructs the indexing engine to ignore errors during data import. This is useful because there are so few documents in the demo data source. For a larger data source, you would set the value to greater than 0.

You should also have set the "dataToExtract":"contentAndMetadata" statement in the configuration parameters. This statement tells the indexer to automatically extract the content from different file formats as well as metadata related to each file.

When content is extracted, you can set ImageAction to extract text from images found in the data source. The "ImageAction":"generateNormalizedImages" tells the indexer to extract text from the images (for example, the word "stop" from a traffic Stop sign), and embed it as part of the content field. This behavior applies to both the images embedded in the documents (think of an image inside a PDF), as well as images found in the data source, for instance a JPG file.

In this preview, "generateNormalizedImages" is the only valid value for "ImageAction".

## Challenge Check indexer status
Once the indexer is defined, it runs automatically when you submit the request. Depending on which cognitive skills you defined, indexing can take longer than you expect. To find out whether the indexer is still running, send a request either via Postman or Code to check the indexer status.

The response tells you whether the indexer is running. Once indexing is finished, the response to the same call (as above) will result in a report of any errors and warnings that occurred during enrichment.

Warnings are common with some source file and skill combinations and do not always indicate a problem. In this lab, the warnings are benign (for example, no text inputs from the JPEG files). You can review the status response for verbose information about warnings emitted during indexing. You can also expect warnings about text been truncated or long words. If the status field has "success", we don't need to worry about any of the warnings.

Since the index columns and skillsets are smaller than in the previous lab, you will also notice the index size is smaller for the same set of indexed documents.

## Verify content
After indexing is finished, run queries that return the contents of individual fields. By default, Azure Search returns the top 50 results. The sample data is small so the default works fine. However, when working with larger data sets, you might need to include parameters in the query string to return more results - you can read [how to page results in Azure Search here](https://docs.microsoft.com/en-us/azure/search/search-pagination-page-layout).

### Challenge
1. As a verification step, query the index for all of the fields.
2. Submit the second query, to verify the metadata created with AI.
### Success Criteria
1. The output is the index schema, with the name, type, and attributes of each field.
2. Please notice that API calls are case sensitive, so it is mandatory to use exactly the same field names of    the index definition.
   
**Note** As you can see, it is possible to return multiple fields via $select using a comma-delimited list.
You can use GET or POST, depending on query string complexity and length. For more information, see [Query using the REST API](https://docs.microsoft.com/en-us/azure/search/search-get-started-powershell).

## For Further Reading
* [Knowledge Store Concept](https://docs.microsoft.com/en-us/azure/search/knowledge-store-concept-intro)
* [Projections Concept](https://docs.microsoft.com/en-us/azure/search/knowledge-store-projection-overview)
* [How to get Started with Knowledge Mining in Azure Search](https://docs.microsoft.com/en-us/azure/search/knowledge-store-create-rest)
* [Knowledge Store End-to-End Example](https://notebooks.azure.com/GraemeMalcolm/projects/azure-search/html/knowledge-store.ipynb)

# Next Step
## Image Skills Lab or Back to Read Me
