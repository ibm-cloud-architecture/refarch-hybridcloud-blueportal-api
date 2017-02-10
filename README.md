#Managing APIs with DevOps in a Hybrid Architecture#

This project provides a reference implementation for managing the lifecycle of APIs using IBM UrbanCode Deploy and the API Connect service on IBM Bluemix.  

###Overview - DevOps with IBM UrbanCode Deploy###

Any DevOps practitioner will aim to simplify the above API lifecycle management through automation.  This will improve the speed, consistency and quality of the environments and builds.  It will also provide support continuous integration and continuous delivery.  

IBM UrbanCode Deploy can be leveraged to automate the deployment of APIs using the API Connect Plug-in.  The API Connect Plug-in communicates with the API Connect service using the API Connect (apic) command line toolkit.  The toolkit can be installed using the Node Package Manager (npm).  

The deployment activities will be defined in IBM UrbanCode Application and Component processes and the processes will be executed by IBM UrbanCode Deploy Agent(s), which are small processes that does the work on behalf of the controller servers.  

![Figure 1 - DevOps for API Connect using IBM UrbanCode Deploy](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/imgs/APIC_UCD_Readme_Figure1.png)
Figure 1 - DevOps for API Connect using IBM UrbanCode Deploy

Figure 1 shows the overall workflow.  A component process defined in IBM UrbanCode Deploy will stage, publish and/or replace a published API definition(s) in the Sandbox, UAT and/or PROD Catalogs.  Once the publishing is complete, developers can go to the developer portal(s) to subscribe to and consume the APIs.  

Before we proceed further, let’s define the different types of API changes and the corresponding Product / API definition version increment rules for Product / API definitions.  

   •	For major (API breaking) changes, defined as a change impacting the API endpoints or major changes to the API implementation, this will result in a major version increment (ie. 1.0.0 -> 2.0.0).  The new version of the API will be published to the UAT and PROD catalogs and the subscriptions will be manually migrated.  Once all the subscriptions are migrated, the older version of the API will be set to a “retired” state. 
   •	For minor (non-breaking) changes, there will be a minor version increment (ie. 1.0.0 -> 1.1.0).  The new version of the API will replace the existing version in the UAT and PROD catalogs.  The subscriptions will be automatically migrated and the older version of the API will be set to the “retired” state. 
   •	For bug fixes and/or patches to the API implementation that do not impact the API interface, no version changes to the API are expected.

###API Connect and IBM UrbanCode Deploy Configuration###

This section explains the relationship between IBM UrbanCode Deploy and API Connect, and specifies the assumptions and requirements for supporting the API lifecycle management.  

The following table provides a mapping between API Connect and IBM UrbanCode Deploy objects.

API Connect	IBM UrbanCode Deploy	Description
Catalog	Environment	A UCD environment represents an API Connect Catalog.  Multiple Products may be deployed to an environment (catalog) as each product is represented by a product component resource in the environment.
Product	Component	One UCD component represents one API Product within API Connect.  The component stores the version artifacts that represent the version of the API Product.
The API Product contains the definition of one or more APIs.    
API Implementation	Component(s)	A UCD Component represents an API Implementation. Each API defined in the Product has a corresponding implementation. The source code that provides the implementation of the API may be from a variety of technologies such as StrongLoop Node.js, java, javascript or other languages.  
Each API implementation is represented by a UCD component.  This component stores the version artifacts that results from the build of the API’s implementation source code.  

#####Assumptions#####
   * When creating the product component version in the Build system (i.e. Jenkins, Rational Team Concert, etc) a set of version properties are required to communicate to IBM UrbanCode Deploy as follows:   
   * The type of change (major | minor | patch) 
   * The version name that is to be replaced by this version.   
   * The subscription plan that the product component version supports
   * When there is a major change, a new branch in the SCM system (Git, Rational Team Concert, etc) is created for that stream.  A major change is defined as an impact on the API endpoints or major changes to the API implementation.  
   * To deploy to the PROD UCD Environment, the product and application component versions require a status of TESTED set on them in order for IBM UrbanCode Deploy to execute the deployment process.  This status can be applied automatically with automated testing implemented in IBM UrbanCode Deploy
   * The API Connect catalog (represented by the IBM UrbanCode environment) may support multiple product version.  For ease of use, the solution outlined in this paper will constrain the major releases per product version to two (N and N+1). 


###Implementation Overview###

Run the reference implementation in a hybrid environment using a local instance of IBM UrbanCode Deploy and IBM Cloud
The following example implementation will walk you through the set up and configuration to manage APIs in a hybrid architecture. 

Note detailed instructions to come!

Step 1: Environment Setup
Pre-requisites
   *	Install IBM UrbanCode Deploy 6.2.2 or later.
   * 	Install the IBM UrbanCode Deploy - API Connect Plug-in. 
   * Install Jenkins 2.19 or later.
   * Install the IBM UrbanCode Deploy plugi
   * Install the GitHub plug
   * Install Node.js version 4.x. 
   * Install API Connect Toolkit using NPM
   * Register for a Bluemix account.

Step 2: Configure Bluemix space(s), provision IBM API Connect service and set up 3 Catalogs (sandbox, UAT and production). 

Step 3: Set up two versions of a sample API implementation and the corresponding API definitions (sourced in GitHub). 

Step 4: Configure the integration between GitHub and Jenkins. 

Step 5: Configure the integration between Jenkins and IBM UrbanCode Deploy

Step 6: Configure IBM UrbanCode Deploy. 

Step 7: Automate the publishing of the API definitions to the API Connect service on IBM Bluemix when the API definitions are changed.
