# Hybrid API DevOps – Activating APIs from Systems of Record (SoRs)

A DevOps platform for APIs provides the ability to streamline and automate the process of developing, building, testing and deploying APIs onto the API runtime cloud platform either when these APIs are organically developed or when they are exposed endpoints to existing backend services.

![Architecture](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/imgs/HybridCloud_API_Architecture.png)

## Architecture Overview

Business leaders recognize a need for a new consumer-accessible service and work with their developers to design the solution. They identify a need for access to Systems of Record (SoRs) as part of the solution. Enterprise developers identify the required facilities and use API management and artifacts from their SoR development processes to expose the needed APIs internally and for use by System of Engagement (SoEs) developers in the Cloud. Having exposed these APIs through their Hybrid Cloud interface, both teams are now able to undertake their particular development processes with relative independence. Notably SoE developers can use agile DevOps methodologies to rapidly develop channel interaction services while relying on the support of stable SoR system APIs.

### Architecture Component Tooling

The specific tools that are used to implement this reference architecture&#39;s components include:

- SCM: GitHub
- Build: Jenkins
- Deploy: IBM UrbanCode Deploy (UCD)
- SoR Runtime for API IBM Integration Bus (IIB)
- SoR API Management:  IBM API Connect (APIC)

## Automating API deployment using a governance model

New or change to existing services and APIs are delivered continuously to a development pipeline. The changes are then handled to support a change and governance model to ensure API subscribers can easily consume the changes. This is critical when the changes delivered require any changes to the end-user subscriber.  In order to support this governance model, we are following the processes outlined in the diagram below:

![API Governance Model](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/imgs/API_Governance_Model.png)

This diagram shows how API implementations and the information published to APIC should be automated to ensure the release governance model is supported. The different types of API changes and the corresponding Product / API definition version increment rules for Product / API definitions include:

- For major (API breaking) changes, defined as a change impacting the API endpoints or major changes to the API implementation, this will result in a major version increment (ie. 1.0.0 -&gt; 2.0.0). The new version of the API will be published to the UAT and PROD catalogs and the subscriptions will be manually migrated. Once all the subscriptions are migrated, the older version of the API will be set to a &quot;retired&quot; state.
- For minor (non-breaking) changes, there will be a minor version increment (ie. 1.0.0 -&gt; 1.1.0). The new version of the API will replace the existing version in the UAT and PROD catalogs. The subscriptions will be automatically migrated and the older version of the API will be set to the &quot;retired&quot; state.
- For bug fixes and/or patches to the API implementation that do not impact the API interface, no version changes to the API are expected.

## Deployment Processes

This diagram reveals the overall DevOps workflow for API implementation and APIC publication using this architecture.  The developer&#39;s role is to create code changes for the implementation as well as the yaml files reflecting the API data. These changes are committed to source control. The developer will then continuous coding and wait for any feedback from the delivery pipeline indicating success or failure to the build, scan, deploy or test portions of their DevOps process. The API management or API product owner delivers changes to their product yaml files reflecting any product level changes and waits for feedback, similarly.

Changes are promoted automatically using a defined set of business rules or through manual activation of the automation. In each case, a promotion to a new environment/catalog includes the latest versions of API code, yaml and the deployment processes. This provides a means to mature the process through testing and validation.

![Deployment Sample](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/imgs/Deployment_Sample.png)

## API Connect and IBM UrbanCode Deploy Configuration

This section explains the relationship between IBM UrbanCode Deploy and API Connect, and specifies the assumptions and requirements for supporting the API lifecycle management.

The following table provides a mapping between API Connect and IBM UrbanCode Deploy objects.

| **API Connect** | **IBM UrbanCode Deploy** | **Description** |
| --- | --- | --- |
| Catalog | Environment | A UCD environment represents an API Connect Catalog. Multiple Products may be deployed to an environment (catalog) as each product is represented by a product component resource in the environment. |
| Product | Component | One UCD component represents one API Product within API Connect. The component stores the version artifacts that represent the version of the API Product. The API Product contains the definition of one or more APIs. |
| API Implementation | Component(s) | A UCD Component represents an API Implementation. Each API defined in the Product has a corresponding implementation. The source code that provides the implementation of the API may be from a variety of technologies such as StrongLoop Node.js, java, javascript or other languages. Each API implementation is represented by a UCD component. This component stores the version artifacts that results from the build of the API&#39;s implementation source code. |

## Implement and Explore this Reference Architecture

### Repository Files

This repository contains the instructions, configuration files and example APIC yaml files to implement the architecture using the products noted above. The are two ways in which you can explore the architecture 1) using IBMs on demand lab environment or 2) implementing in your own environment. To accelerate your experience, this repository contains the assets to quickly establish an environment, implement the architecture and a guide to exploring the results. This repository contains:

- Jenkins job archive containing build job for APIC content: [/jenkins/acme-bank.tar.Z](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/jenkins/acme-bank.tar.Z)
- API Product Definition YAML: [acme-bank-product.yaml](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/acme-bank-product.yaml)
- API Definition YAML: [acme-bank.yaml](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/acme-bank.yaml)
- IBM UrbanCode Deploy Application JSON: [/ucd-application/acme-bank.json](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/ucd-application/acme-bank.json)
- Lab for exploring the architecture  [HybridDevOpsForAPIC.pdf](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/HybridDevOpsForAPIC.pdf)

### Options to implement the architecture

#### 1. Run the reference architecture in a IBM hosted lab environment.

Click this [link](https://bluedemos.com/app/home/session/578/0UXM81F63PFE5PE6O1GH2AWCDI6SJW6U6T03B8I4BBR8TVB1EMItmco6lhhox1i6) to experience this implementation using our cloud hosted lab. A detailed set of [instructions](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/HybridDevOpsForAPIC.pdf) is available to begin your journey. No installation is required as this lab runs in a public cloud.

- Locate the &quot;HybridDevOpsForAPIC.pdf&quot; document in this repository and use the steps outlined to explore this architecture.

#### 2. Run the reference architecture in your local environment.

##### Step 1: Setup your local environment  

- Register for a [Bluemix](https://bluemix.net/registration) account
- [Install Node.js v4.x](https://nodejs.org/en/)
- [Install Git](https://git-scm.com/)
- [Install shyaml](https://github.com/0k/shyaml) (YAML parser)
- [Install UrbanCode Deploy on premises](https://www.ibm.com/support/knowledgecenter/SS4GSP_6.2.3/com.ibm.udeploy.install.doc/topics/install_ch.html) or [Use the SaaS version](https://www.ibm.com/us-en/marketplace/application-release-automation)
- [Install UrbanCode Deploy Agent](https://www.ibm.com/support/knowledgecenter/SS4GSP_6.2.2/com.ibm.udeploy.install.doc/topics/agent_install_ov.html)
- [Install API Connect plugin for UCD](https://developer.ibm.com/urbancode/plugin/ibm-api-connect/)
- [Deploy API Connect Service in Bluemix](https://console.ng.bluemix.net/catalog/services/APIConnect/) – or - [Install API Connect on premises](https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.install.doc/overview_installing_apimgmt.html)
- [Install API Connect Toolkit](https://www.ibm.com/support/knowledgecenter/en/SSFS6T/com.ibm.apic.toolkit.doc/tapim_cli_install.html)
- [Install Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins) and [Jenkins plugin for UCD](https://developer.ibm.com/urbancode/plugin/jenkins-2-0/)

##### Step 2: Create a repository clone

- Clone this GIitHub repository:
  - [https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api.git](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api.git)

##### Step 3: Create API Connect Catalogs

- Use the [General instructions to create API Connect catalogs:](https://www.ibm.com/support/knowledgecenter/en/SSFS6T/com.ibm.apic.apionprem.doc/create_env.html)

- andbox (default Catalog)
- UAT
- PROD

![API Connect Catalogs](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/imgs/APIC_Catalogs.png)

##### Step 4: Configure pipeline tools and integrations

- [Create Jenkins Job from disk](https://wiki.jenkins-ci.org/display/JENKINS/Administering+Jenkins#AdministeringJenkins-Moving%2Fcopying%2Frenamingjobs)
  - Copy the file [acme-bank.tar.Z](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/jenkins/acme-bank.tar.Z) into the file system where Jenkins is installed.
  - Extract the contents of &quot;acme-bank.tar.Z&quot; into the &lt;Jenkins installation dir&gt;/jobs/
  - Navigate to the Jenkins feature Manage Jenkins &gt; Configure Jenkins and run &quot;reload from disk&quot; to automatically create the acme bank job.

- [Configure Jenkins and GitHub integration](https://jenkins.io/solutions/github/)
  - Add a webhook to the GitHub repository using the Jenkins URL (ie https://&lt;Jenkins server&gt;:&lt;port&gt;/github-webhook/).

![Git Webhook](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/imgs/GitHub_Repo_Webhook.png)

- [Configure IBM UrbanCode Deploy and Jenkins integration](http://www-01.ibm.com/support/docview.wss?uid=swg21664334)
  - Add the UCD server details to the Jenkins&#39; UCD configuration page and test the connection.

![Jenkins UCD Config](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/imgs/Jenkins_UCD_Config.png)

##### Step 5: Configure IBM UrbanCode Deploy

- [Import configuration](https://www.ibm.com/support/knowledgecenter/SS4GSP_6.2.3/com.ibm.udeploy.doc/topics/app_import.html)
  - Locate the file [acme-bank.json](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/ucd-application/acme-bank.json) and import it to the server file system where UCD is installed.
  - Locate the UCD import feature in Applications &gt; Import. And run the import of the acme-bank.json.

![UCD JSON Import](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/imgs/UCD_Import_JSON.png)

- Update Environment Information
  - Update each UCD Environment properties to reference corresponding API Connect catalog properties.
    - Enter the URL to APIC you are using
    - APIC Username
    - APIC Tooklit path
    - APIC Server Org, Catalog ID and Product definition

![UCD Env Prop](https://github.com/ibm-cloud-architecture/refarch-hybridcloud-blueportal-api/blob/master/imgs/UCD_Env_Prop.png)

##### Step 6: Publish API and Product to API Connect

- Locate the &quot;HybridDevOpsForAPIC.pdf&quot; document in this repository  and use the steps outlined to explore this architecture.
