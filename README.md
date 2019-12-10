# Capstone Project: Azure Cognitive Search - Knowledge Mining
## Knowledge Mining Hackathon - Build a Cognitive Search Solution for business documents using Microsoft AI Platform


## About this Hackathon 
In this hack, you will create an enterprise search solution by applying knowledge mining to business documents like contracts, memos, presentations, images and forms. You will use Microsoft Azure AI technology to extract insights from unstructured data and expose the results in a Bot interface.

## Goals
At the end of this hackathon you will have learned: 
* **What** Azure Cognitive Search is
* **How** to implement a Cognitive Search Solution
* **Why** this technology can be useful for any company
* **When** to use this solution for demos, POCs and other business scenarios
The hands-on hack will teach you how to use Microsoft Azure Search combined with Microsoft Cognitive Services for entity recognition, image analysis, text translation, form recognizer and indexed search on enterprise business documents. This approach uses Artificial Intelligence to create an advanced search experience.

In this hackathon we will cover these key concepts:
1.	Fundamentals of Azure Search and its capabilities
2.	Microsoft Cognitive Search and its key business scenarios
3.	Building an enrichment data pipeline for search using predefined and custom skillsets:
    * Text skills like entity recognition, language detection, text manipulation and key phrase extraction
    * Image skills like OCR
    * Content moderation skills to detect documents with incompliant content
4.	Use the enriched data for an advanced search experience for business documents within an enterprise.
5.	Expose the knowledge mining solution using a bot interface for document search and consumption.
6.	Create custom skill that leverages the Form Recognizer Service to get insights from the form-based        data by extracting key-value pairs that describe each form field and the value it contains.

### Architecture 
* Solution Architecture ![alt text](https://github.com/frdogbey/AI-Powered-Knowledge-Management-Capstone-Project-1/blob/master/resources/images/readme/architecture.png)

### Microsoft AI Tech 
* Technologies Covered ![alt text](https://github.com/frdogbey/AI-Powered-Knowledge-Management-Capstone-Project-1/blob/master/resources/images/readme/tech-map.png)

### Industry application
Intelligent search is relevant to many major industries. Some are listed below.
1.	Retail and health care industries employ chatbots with advanced multi-language support capabilities       to service their customers.
2.	Retail, Housing and Automotive industries for sales/listing.
3.	Law firms and legal departments can use this technology to enforce compliance or improve search           capabilities.
4.	Travel Agencies that maintains collections of scanned travel insurance application forms submitted by     their clients.

### Pre-requisites
1.	Fundamental working knowledge of Azure Portal, Azure Functions and Azure Search
2.	Familiarity with Visual Studio and minimum C# knowledge
3.	Familiarity with Azure Bots and Microsoft Bot Framework v4
4.	Familiarity with [Postman](https://www.getpostman.com/)

If you do not have any of the above pre-requisites, please find below links
1.	To Read (10 minutes): [Visual Studio Tutorial](https://docs.microsoft.com/en-us/visualstudio/get-started/visual-studio-ide?view=vs-2019)
2.	To Read (8 minutes): Azure [Bot Service Overview](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
3.	To Read (4 minutes): [Azure Functions Overview](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview)
4.	To Read (10 minutes): [Azure Search Overview](https://docs.microsoft.com/en-us/azure/search/search-what-is-azure-search)
5.	To Read (7 minutes): [Postman Tutorial](https://docs.microsoft.com/en-us/azure/search/search-get-started-postman)
6.	To Do (30 minutes): [C# Quickstart](https://docs.microsoft.com/en-us/dotnet/csharp/tutorials/intro-to-csharp/)
7.	Microsoft C# (Visual Studio) - [how to use it](https://docs.microsoft.com/en-us/visualstudio/ide/quickstart-ide-orientation?view=vs-2019), C# (intermediate level - you can learn [here](https://channel9.msdn.com/Series/CSharp-Fundamentals-for-Absolute-Beginners?l=Lvld4EQIC_2706218949) and [here](https://docs.microsoft.com/en-us/dotnet/csharp/tutorials/intro-to-csharp/))
8.	Visual Studio Code - [Visual Studio Code](https://code.visualstudio.com/)
9.	Node/JavaScript - [Node.js](https://nodejs.org/en/)
10.	Python - [Python Extension for Visual Studio Code, Getting started with Python, Azure For Python           Developers](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
11.	Azure Functions - After installing the language-specific extensions above, install the [Azure              Functions extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)


### Pre-Setup before you attend the class Mandatory
1.	To Create: You need a Microsoft Azure account to create the services we use in our solution. You can      create a [free account](https://azure.microsoft.com/en-us/free/), use your MSDN account or any other      subscription where you have permission to create services.
2.	To Install: [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) or later, if any, including      the Azure development workload
3.	To Install: [Postman](https://www.getpostman.com/). To call the labs APIs
4.	To Install: [Bot Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases), use the '        .exe' file from release 4.1.0 or newer
5.	To Install: [Git for Windows](https://gitforwindows.org/) or any other git app you prefer

### Capstone Project Details
Primary Audience: Azure AI Developers, Solution Architects. Secondary Audience: Any professional interested in learning AI.

### Level
This content is designed as an intermediate to advanced level course for AI developers and/or architects.

### Type
This hackathon, in its full form, is designed to be taught in-person but you can also use the materials in a self-paced fashion. There are assignments and multiple reference links throughout the materials that support the concepts and skills you will learn.

### Length
Full Course classroom training: 16 hours

### Course Modules
1.	**Introduction** – **Presentation** overview of Azure Search, Cognitive Search, business scenarios        and industry specific applications.
2.	**Architecture** – **Solution Architecture** for building enterprise search solution.
3.	**Lab 1** - Azure - **Environment Creation**
4.	**Lab 2** - Azure Search - **Indexing Blob Storage**
5.	**Lab 3** - Cognitive Search – **Text Skills**
6.	**Lab 4** - Cognitive Search – **Image Skills**
7.	**Lab 5** - Cognitive Search – **Custom Skills**
8.	**Lab 6** - Build and Integrate a **Bot** with Cognitive Search API
9.	**Lab 7 - **Architecture Design Session**
10. **Lab 8** - Finding Your Form Using **Form Recognizer** with Custom Skill

**Note**: Once you've completed the labs, we recommend deleting the resource group (and all the resources in it) to avoid incurring extra charges, unless you want to use this solution as a tool for demos and POCs.