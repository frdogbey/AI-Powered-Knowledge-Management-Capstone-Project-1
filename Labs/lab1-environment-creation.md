# Capstone Project: Lab1-Environment Creation.

# Environment Creation
In this lab, you will create an Azure Search service and a storage account. We recommend keeping both in a new and unique resource group, to make it easier to delete at the end of the workshop (if you want to). We will also upload the data to a blob storage within the storage account.

## Step 1 – Download Lab Material or Clone the Repo
Cloning the repo will download all the training materials to your computer, including the dataset, the slides and the code for the Bot project. **The cloning of the repository will use close to 185 MB in total**. If you have any git tool in your computer, you can run git clone it in your command prompt. Or simply follow the tasks below:

### Tasks - cloning the repo using zip file download
1.	Open a browser window to the GitHub repository
2.	Select **Clone or download**, then select **Download Zip**.
3.	Extract the zip file to your local machine, be sure to keep note of where you have extracted the files.       You should now see a set of folders:


## Step 2 - Create the Azure Search service
1.	Go to the [Azure portal](https://portal.azure.com) and sign in with your Azure account.
2.	Create a new resource group, click Resources groups, then click Add. Select a subscription, type a name       for the group, such as **INIT-kmb** and then select a region. Click **Review + Create**, then click           **Create**
3.	In the resource group, click **Add**. Search for **Azure Search**, then select Azure Search, then click       **Create**. In addition to facilitating organization and visualization in the portal, using a single          resource group helps you, if necessary at the end of the training, remove all services created. If you        want to keep this solution up and running, for demos and POCs in minutes with your own data, this             resources cleaning isn't necessary.
4.	Click **Create a resource**, search for Azure Search, and click Create. See Create an Azure Search            service in the portal if you are setting up a search service for the first time, and use the bullet point     list below for the details you will use to fill out the details for the Azure Search service.

    * Azure portal search service creation ![alt text](https://github.com/frdogbey/AI-Powered-Knowledge-Management-Capstone-Project-1/blob/master/resources/images/lab-environment-creation/create-service-full-portal.png)
 
1.	Ensure your newly created resource group is selected.
2.	For the URL, type your service name, choose a name that you can easily remember. We will use it many          times in the labs.

**Note**: The name of the service in the screenshots of this lab won't be available, you must create your own service name.
1.	For **Location**, choose one of [these](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=search) regions. Suggested regions are:
* West Central US
* South Central US
* North Central US
* East US
* East US 2
* West US
* West US 2
* Canada Central
* West Europe
* UK South
* North Europe
* Brazil South
* East Asia
* Southeast Asia
* Central India
* Australia East
  
2.	For **Pricing tier**, select **Standard**. 
**NOTE**: For deeper information on Azure Search pricing and limits, review [pricing]((https://azure.microsoft.com/en-us/pricing/details/search/)) and [capacity](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity).

1.	Click **Review + Create**, then click **Create**
2.	Once the service is created, under **Settings**, click **Keys**
3.	Copy the **Primary admin key** to notepad or similar text editor for use later in the labs.

* Service creation Info ![alt text](https://github.com/frdogbey/AI-Powered-Knowledge-Management-Capstone-Project-1/blob/master/resources/images/lab-environment-creation/create-search-collect-info.png)

**Note** Azure Search must have 2 replicas for read-only SLA and 3 replicas for read/write SLA. This is not addressed in this training. For more information, click [here](https://azure.microsoft.com/en-us/support/legal/sla/search/v1_0/)

## Step 3 - Create the Azure Blob service and upload the dataset
The enrichment pipeline pulls from Azure data sources. Source data must originate from a supported data source type of an [Azure Search indexer](https://docs.microsoft.com/en-us/azure/search/search-indexer-overview). For this exercise, we use blob storage to showcase multiple content types.

## Setting Up a Development Environment
Before you begin your development work, you should set up your development environment. You may also use Visual Studio Code editor for this hackathon. After installing Visual Studio Code, install the following language-specific extensions for the language you plan to use:

## C#
* [NET Core SDK](https://dotnet.microsoft.com/download)
* [C# Extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

## Python
* [Python Extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
After installing the language-specific extensions, install the [Azure Functions extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions). You will use this in some of the later challenges.

Most of the operations to create and modify Azure Search entities involve submitting JSON requests to a REST interface. You can write code in your preferred language to do this, or you can use [Postman](https://www.getpostman.com/downloads/) to create and submit collections of REST requests.

## Hints
* You will need to be able to set-up the azure services that is scoped in the solution architecture and use     these to process and extract from the business cocuments like contracts, memos, presentations, images and     forms.
* Ensure your Azure Cognitive Services Account is created and read to go. Ensure the pricing tier is set to     S0. A Cognitive Services resource is needed in order to enrich more than 20 documents per day in Azure        Search indexing.
* Determine where and how to make the customer's unstructured data available so as to extract insights from     them and later expose the results in a Bot interface.    
* For performance, ensure storage account type is Standard
* For account kind, select StorageV2
* For replication, select Locally-redundant storage LRS
* You will need to create 2 containers.
* Container1 could be named projections and designate the Access Type as Container.
* Container2 could be named basicdemo and designate the Access Type as Container.
* Now ensure all the files you cloned from the repo are uploaded to your respective container

**Note** You can also use the [Azure Storage Explorer](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-storage-explorer) to upload files. If you use the Storage Explorer, be careful not to create another folder level. This hackathon is created with the assumption that all of the data is located in the root folder of the container.

## Success Criteria
To complete this environment set-up challenge successfully, you must:
* Ensure that 21 files were uploaded to the basicdemo container
* Take note of your Access keys and save key1 to a notepad or similar text editor.

## Next Step
## Azure Search Lab 

