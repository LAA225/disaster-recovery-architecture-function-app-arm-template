# disaster-recovery-architecture-linked-arm-template

This arm template seeks to deploy the following achitecture:

![architecture diagram](images/architecture diagram.png)

The function apps contain the following resources:
* storage account
* serverfarm
* Web/site (kind: windows functionapp)
* insight component (for monitoring)

The traffic Manger contains the following resources:
* Traffic Manager


## How it works

Only need to deploy *maindeploy.json* with optional parameters which deploys everything else automatically.

The maindeploy.json is the parent template file that contains linked template deployments:
* **Primary Function Deployment:** This linked template deploys the primary function app using template file *fundeploy.json* and it's parameter file *primarydeploy.parameters.json*

* **Secondary Function Deployment:** This linked template deploys the secondary function app using the template file *fundeploy.json* and it's parameter file *secondarydeploy.parameters.json*

* **Traffic Manager Deployment:** This linked template deploys the traffic manager using the template file *trafficdeploy.json*. it gets it's parameters from *maindeploy.json*

The parent template file takes 5 parameters:
* **RgName1:** Name of the resource group to deploy the primary function app to
  * type: string
  * defaultValaue: rg-primary-app
  
* **RgName2:** Name of the resource group to deploy the secondary function app to
  * type: string
  * defaultValaue: rg-primary-ap
  
* **RgName3:** Name of the resource group to deploy the traffic manager to
  * type: string
  * defaultValaue: rg-primary-ap
  
* **primaryPriority:** Priority to be assigned to the primary function app
  * type: int
  * defaultValaue: 4
  
* **secondaryPriority:** Priority to be assigned to the secondary function app
  * type: int
  * defaultValaue: 8
 
 
## Usage

#### Prerequisites
1. Azure Subscription on Azure Portal
2. Powershell
3. Az module

#### Steps
1. Sign in to Azure portal through powershell using the command:\
          `Connect-AzAccount`\
This gives a link to an http link and a code. Go to the link and enter the code to sign in.

2. Create the necessary Resource groups you want to deploy the architecture in using:\
     `New-AzResourceGroup -Name <name of rg> -location <location of rg>`\
