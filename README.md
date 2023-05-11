# MailRisk DataConnector for Azure Sentinel

This repository is an Azure Function which checks the Secure Practice API for recent MailRisk events, and if found posts recent emails to Azure Sentinel.  


## Manual deployment 
If you don't want to wait for the official Data Connector it is possible to download this repository and publish it as a Azure Function to your Azure account using VS code. The deployment instructions below were retrieved from: [Use Azure Functions to connect Microsoft Sentinel to your data source](https://docs.microsoft.com/en-us/azure/sentinel/connect-azure-functions-template?tabs=MPY).

1. Prepare [VS code](https://docs.microsoft.com/nb-no/azure/azure-functions/create-first-function-vs-code-python?WT.mc_id=Portal-fx#configure-your-environment) for Azure function deployment. Make sure you have the following requirements in place:
   - An Azure account with and active subscription.
   - The [Azure Functions Core Tools](https://docs.microsoft.com/nb-no/azure/azure-functions/functions-run-local?tabs=v4%2Cwindows%2Ccsharp%2Cportal%2Cbash%2Ckeda#install-the-azure-functions-core-tools) version of 3.x or above.
   - The [Azure Functions exstension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) for Visual Studio Code.

2. [Download the ZIP of this repository](https://github.com/securepractice/mailrisk-sentinel-connector/archive/refs/heads/master.zip). Extract ZIP to your local development computer.

3. Open VS Code. Choose File in the main menu and select Open Folder and select the top level folder from extracted files..

4. Choose the Azure icon in the Activity bar, then in the Azure: Functions area, choose the Deploy to function app button. If you aren't already signed in, choose the Azure icon in the Activity bar. Then, in the Azure: Functions area, choose Sign in to Azure If you're already signed in, go to the next step.

5. Provide the following information at the prompts:
  - Select folder: Choose a folder from your workspace or browse to one that contains your function app.
  - Select Subscription: Choose the subscription to use.
  - Select Create new Function App in Azure (do not choose the Advanced option)
  - Enter a globally unique name for the function app: Type a name that is valid in a URL path. The name you type is validated to make sure that it's unique in Azure Functions.
  - Select a runtime: Choose Python 3.9.
  - Select a location for new resources. We recommend choosing the same region where Microsoft Sentinel is located.

6. Deployment will begin. A notification is displayed after your function app is created and the deployment package is applied.

7. Go to Azure Portal for the Function App configuration.

8. In the Function App, select the Function App Name and select Configuration.

9. In the Application settings tab, select + New application setting.
    - Add each of the following application settings individually, with their respective string values (case-sensitive):
      - `WORKSPACE_ID`
      - `WORKSPACE_KEY`
      - `API_KEY` (Secure Practice API Key)
      - `API_SECRET` (Secure Practice API Secret)
    - Workspace ID and Primary Key can be found in the Azure Portal, and you should find the workspace that is connected to Azure Sentinel. Typically found under Log Analytics workspace -> Agents management
    - The Secure Practice API key and secret are created in the [settings in the admin portal](https://manage.securepractice.co/settings/security). 
      - If you have lost your API secret, you can generate a new key pair.
        <span style="color:red">WARNING:</span> Any services using the old key pair will stop working.
10. Once all application settings have been entered, click Save.



## Installation
For local development in Visual Studio Code:
1. Install Azure Functions and Azurite extensions. Make sure to start Azurite by running `azurite -s -l c:\azurite -d c:\azurite\debug.log`.
2. Create the file local.settings.json which should contain:
```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "API_KEY": "{SECURE_PRACTICE_API_KEY}",
    "API_SECRET": "{SECURE_PRACTICE_API_KEY}",
    "BASE_URL": "https://api.mailrisk.com/v1/",

    "WORKSPACE_ID": "{AZURE_WORKSPACE_ID}",
    "WORKSPACE_KEY": "{AZURE_WORKSPACE_KEY}",
    "LOG_TYPE": "MailRiskEmails",

    "VERIFY_CERTIFICATE": "False"
  }
}
```
3. Run the Azure Function by clicking `F5`.
4. Trigger the timerTrigger manually by going to Azure extenstion tab -> Local Project -> Functions -> Left click on MailRiskSentinelIntegration and click `Execute Function Now...`

## Usage
- The `mailrisk.py` file contains helpful functions for interacting with MailRisk.
- The `sentinel_api.py` file contains helpful functions for posting to Sentinel
- The `sentinel_integration.py` file contains logic for retrieving and posting emails to Azure Sentinel.
- The `MailRiskSentinelIntegration/__init__.py` file is where the Azure Function triggers the python code.




## Contributions

### Code guidelines

_ marks functions and classes as private

## Feedback or issues

If you have any feedback or issues, be kind and create a new [issue](https://github.com/securepractice/mailrisk-sentinel-connector/issues) or contact us via our [support page](https://securepractice.co/support).
## License
See the [LICENSE](https://github.com/securepractice/mailrisk-sentinel-connector/blob/master/LICENSE) file for license rights and limitations (MIT).
