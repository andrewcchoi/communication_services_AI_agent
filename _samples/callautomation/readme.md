|page_type| languages                               |products
|---|-----------------------------------------|---|
|sample| <table><tr><td>Python</tr></td></table> |<table><tr><td>azure</td><td>azure-communication-services</td></tr></table>|

# Call Automation - Quick Start Sample

In this quickstart, we cover how you can use Call Automation SDK to make an outbound call to a phone number and use the newly announced integration with Azure AI services to play dynamic prompts to participants using Text-to-Speech and recognize user voice input through Speech-to-Text to drive business logic in your application.

# Design

![design](./data/OutboundCallDesign.png)

## Prerequisites

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- A deployed Communication Services resource. [Create a Communication Services resource](https://docs.microsoft.com/azure/communication-services/quickstarts/create-communication-resource).
- A [phone number](https://learn.microsoft.com/en-us/azure/communication-services/quickstarts/telephony/get-phone-number) in your Azure Communication Services resource that can make outbound calls. NB: phone numbers are not available in free subscriptions.
- Create Azure AI Multi Service resource. For details, see [Create an Azure AI Multi service](https://learn.microsoft.com/en-us/azure/cognitive-services/cognitive-services-apis-create-account).
- Create and host a Azure Dev Tunnel. Instructions [here](https://learn.microsoft.com/en-us/azure/developer/dev-tunnels/get-started)
- [Python](https://www.python.org/downloads/) 3.7 or above.
- (Optional) A Microsoft Teams user with a phone license that is `voice` enabled. Teams phone license is required to add Teams users to the call. Learn more about Teams licenses [here](https://www.microsoft.com/microsoft-teams/compare-microsoft-teams-bundle-options).  Learn about enabling phone system with `voice` [here](https://learn.microsoft.com/microsoftteams/setting-up-your-phone-system).   You also need to complete the prerequisite step [Authorization for your Azure Communication Services Resource](https://learn.microsoft.com/azure/communication-services/how-tos/call-automation/teams-interop-call-automation?pivots=programming-language-javascript#step-1-authorization-for-your-azure-communication-services-resource-to-enable-calling-to-microsoft-teams-users) to enable calling to Microsoft Teams users.

## Before running the sample for the first time

1. Open an instance of PowerShell, Windows Terminal, Command Prompt or equivalent and navigate to the directory that you would like to clone the sample to.
2. git clone `https://github.com/Azure-Samples/communication-services-python-quickstarts.git`.
3. Navigate to `callautomation-outboundcalling` folder and open `main.py` file.

### Setup the Python environment

Create and activate python virtual environment and install required packages using following command 
```
pip install -r requirements.txt
```

### Setup and host your Azure DevTunnel

[Azure DevTunnels](https://learn.microsoft.com/en-us/azure/developer/dev-tunnels/overview) is an Azure service that enables you to share local web services hosted on the internet. Use the commands below to connect your local development environment to the public internet. This creates a tunnel with a persistent endpoint URL and which allows anonymous access. We will then use this endpoint to notify your application of calling events from the ACS Call Automation service.

```bash
devtunnel user login
devtunnel create --allow-anonymous
devtunnel port create -p 8080
devtunnel host
```

### Configuring application

Open `main.py` file to configure the following settings

1. `ACS_CONNECTION_STRING`: Azure Communication Service resource's connection string.
2. `ACS_PHONE_NUMBER`: Phone number associated with the Azure Communication Service resource. For e.g. "+1425XXXAAAA"
3. `TARGET_PHONE_NUMBER`: Target phone number to add in the call. For e.g. "+1425XXXAAAA"
4. `CALLBACK_URI_HOST`: Base url of the app. (For local development use dev tunnel url)
5. `COGNITIVE_SERVICES_ENDPOINT`: Cognitive Service Endpoint
6. `TARGET_TEAMS_USER_ID`: (Optional) update field with the Microsoft Teams user Id you would like to add to the call. See [Use Graph API to get Teams user Id](https://learn.microsoft.com/azure/communication-services/how-tos/call-automation/teams-interop-call-automation?pivots=programming-language-python#step-2-use-the-graph-api-to-get-microsoft-entra-object-id-for-teams-users-and-optionally-check-their-presence).  Uncomment the below snippet in main.py to enable Teams Interop scenario.

```python
call_connection_client.add_participant(target_participant = CallInvite(
    target = MicrosoftTeamsUserIdentifier(user_id=TARGET_TEAMS_USER_ID),
    source_display_name = "Jack (Contoso Tech Support)"))
```

## Run app locally

1. Navigate to `callautomation-outboundcalling` folder and run `main.py` in debug mode or use command `python ./main.py` to run it from PowerShell, Command Prompt or Unix Terminal
2. Browser should pop up with the below page. If not navigate it to `http://localhost:8080/` or your dev tunnel url.
3. To initiate the call, click on the `Place a call!` button or make a Http get request to `https://<CALLBACK_URI_HOST>/outboundCall`
