  # PAN-OS Logic Apps connector and playbook templates

  <img src="./PaloAltoCustomConnector/PAN-OS_CustomConnector.png" alt="drawing" width="20%"/>

## Table of Contents

1. [Overview](#overview)
1. [Deploy Custom Connector + 3 Playbook templates](#deployall)
1. [Authentication](#authentication)
1. [Prerequisites](#prerequisites)
1. [Deployment](#deployment)
1. [Post Deployment Steps](#postdeployment)



<a name="overview">

# Overview

PAN‑OS is the software that runs all Palo Alto Networks next-generation firewalls. This integration will allow your SOC to leverage automation to block traffic to/from specific IP or URL as a response to Azure Sentinel incidents.

**PAN-OS custom connector** includes [various actions](./PaloAltoCustomConnector#actions-supported-by-palo-alto-custom-connector) which allow you to create your own playbooks from scratch. In addition to the connector, there are 3 OOTB **playbooks templates** which leverage it so you can start automating Blocking of IPs an URLs with minimum configurations and effort. The OOTB scenarios are leveraging **address objects groups**, which are pre-configured to be refferenced to Security Policy rules. The playbooks will add IPs and URLs as address objects to these groups, so the policies will apply on them.


## Deploy Custom Connector + 3 Playbook templates
This package includes:
* Custom connector for PAN-OS.
* Three playbook templates leverage PAN-OS custom connector.

You can choose to deploy the whole package connector + all three playbook templates, or each one seperately from it's specific folder.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fdev.azure.com/SentinelAccenture/_git/Sentinel-Accenture%20Logic%20Apps%20connectors?version=GBPaloAlto-PAN-OS&path=%2Fazuredeploy.json) [![Deploy to Azure](https://aka.ms/deploytoazuregovbutton)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fdev.azure.com/SentinelAccenture/_git/Sentinel-Accenture%20Logic%20Apps%20connectors?version=GBPaloAlto-PAN-OS&path=%2Fazuredeploy.json)


# PAN-OS connector documentation 

<a name="authentication">

## Authentication
Authentication methods this connector supports- [API Key authentication](https://paloaltolactest.trafficmanager.net/restapi-doc/#tag/key-generation)

<a name="prerequisites">

### Prerequisites for using and deploying Custom Connector
1. PAN-OS service end point should be known. (e.g. https://{paloaltodomain})
2. Generate an API key. [Refer this link on how to generate the API Key](https://paloaltolactest.trafficmanager.net/restapi-doc/#tag/key-generation)
3. Address group should be created for PAN-OS for blocking/unblocking address objects and this address group should be used while creating playbooks.


<a name="deployment">

### Deployment instructions 
1. Deploy the Custom Connector and playbooks by clicking on "Deploy to Azure" button. This will take you to deploying an ARM Template wizard.
2. Fill in the required parameters for deploying custom connector and playbooks:

| Parameter | description |
|----------------|--------------|
|**Custom connector name**| Enter the Custom connector name (e.g. contoso PAN-OS connector). This is the name that will appear in the connectors gallery in the Logic Apps designer.
|**Service Endpoint**|  Enter the PAN-OS service end point (e.g. https://{yourPaloAltoDomain})|
|**Enrich Incident Playbook Name**|  Give a name to the enrichment playbook  (e.g. PaloAlto-PAN-OS-GetURLCategoryInfo playbook)|
|**PaloAlto-PAN-OS-BlockIP Playbook Name**| Enter name for the response playbook which blocks IPs name here (e.g. PaloAlto-PAN-OS-BlockIP)|
|**PaloAlto-PAN-OS-BlockURL Playbook Name**|  Enter name for the response playbook which blocks URLs (e.g. PaloAlto-PAN-OS-BlockURL)|
|**Teams GroupId**| Enter the Teams channel id to send the adaptive card in both response playbooks<br>|
| **Teams ChannelId**| Enter the Teams Group id to send the adaptive card in both response playbooks <br>[Refer the below link to get the channel id and group id](https://docs.microsoft.com/en-us/powershell/module/teams/get-teamchannel?view=teams-ps)<br>|
|**Predefined address group name**|Enter the pre-defined address group name which blocks IPs or URLs||
<br><br>

<a name="postdeployment">

### Post-Deployment instructions 
#### a. Authorize connections
Once deployment is complete, you will need to authorize each connection.
1.	Click the Azure Sentinel connection resource
2.	Click edit API connection
3.	Click Authorize
4.	Sign in
5.	Click Save
6.	Repeat steps for other connections such as Teams connection and PAN-OS API  Connection (For authorizing the PAN-OS API connection, API Key needs to be provided)
#### b. Configurations in Sentinel
1. In Azure sentinel analytical rules should be configured to trigger an incident with risky user account. 
2. Configure the automation rules to trigger the playbooks.