# AWS-SSM-StopManagedInstance

## Summary
This playbook can be used by SOC Analysts to stop malicious or compromised EC2 instances in AWS. This playbook uses AWS Systems Manager API to stop the EC2 instances. The playbook can be triggered from an incident in Microsoft Sentinel. The playbook takes the Hostnames and Private IP addresses from the incident entities and stops the EC2 instances using the Instance IDs. The playbook also adds a comment to the incident with the list of instances that were stopped.

Playbook performs the following actions:

1. Get the Hostnames and Private IP addresses from incident entities.
2. Get the Instance IDs of Managed EC2 instances using the Hostnames and Private IP Addresses.
3. Stop the EC2 instances using the Instance IDs.
4. Add a comment to the incident with the list of instances that were stopped.

<img src="./images/AWS-SSM-StopManagedInstance_light.jpg" width="50%"/><br>
<img src="./images/AWS-SSM-StopManagedInstance_IncidentComment.jpg" width="50%"/><br>

### Prerequisites

1. Prior to the deployment of this playbook, [AWS Systems Manager API Function App Connector](../../CustomConnector/AWS_SSM_FunctionAppConnector/) needs to be deployed under the same subscription.
2. Refer to [AWS Systems Manager API Function App Connector](../../CustomConnector/AWS_SSM_FunctionAppConnector/readme.md) documentation to obtain AWS Access Key ID, Secret Access Key and Region. 

### Deployment instructions

1. To deploy the Playbook, click the Deploy to Azure button. This will launch the ARM Template deployment wizard.
2. Fill in the required parameters:
    * Playbook Name
    * Functions App Name - Name of the Function App where the AWS Systems Manager API Function App Connector has been deployed.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAzure-Sentinel%2Fmaster%2FSolutions%2FAWSAthena%2FPlaybooks%2FAWSAthenaPlaybooks%2FAWSAthena-GetQueryResults%2Fazuredeploy.json) [![Deploy to Azure](https://aka.ms/deploytoazuregovbutton)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAzure-Sentinel%2Fmaster%2FSolutions%2FAWSAthena%2FPlaybooks%2FAWSAthenaPlaybooks%2FAWSAthena-GetQueryResults%2Fazuredeploy.json)

### Post-Deployment instructions

#### a. Authorize connections

Once deployment is complete, authorize each connection.

1. Click the Microsoft Sentinel connection resource
2. Click edit API connection
3. Click Authorize
4. Sign in
5. Click Save
6. Repeat steps for other connections

#### b. Configure Playbook in Microsoft Sentinel
1. In Microsoft sentinel, analytical rules should be configured to trigger an incident that contains IP Addresses or Hostnames. In the *Entity mapping* section of the analytics rule creation workflow, IP Address should be mapped to **Address** identifier of the **IP** entity type and Hostname should be mapped to **Hostname** identifier of the **Host** entity type. Check the [documentation](https://docs.microsoft.com/azure/sentinel/map-data-fields-to-entities) to learn more about mapping entities.
2. Configure the automation rules to trigger the playbook. Check the [documentation](https://docs.microsoft.com/azure/sentinel/tutorial-respond-threats-playbook) to learn more about automation rules.

#### c. Assign Playbook Microsoft Sentinel Responder Role
1. Select the Playbook (Logic App) resource
2. Click on Identity Blade
3. Choose System assigned tab
4. Click on Azure role assignments
5. Click on Add role assignments
6. Select Scope - Resource group
7. Select Subscription - where Playbook has been created
8. Select Resource group - where Playbook has been created
9. Select Role - Microsoft Sentinel Responder
10. Click Save

#### d. Function App Settings Update Instructions
Refer to [AWS Systems Manager API Function App Connector](../../CustomConnector/AWS_SSM_FunctionAppConnector/readme.md) documentation for Function App **Application Settings (Access Key ID, Secret Access Key and Region)** update instruction.

#  References
- [AWS Systems Manager API Documentation](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteDocument.html)
- [AWS Systems Manager User Guide](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html)
- [AWS Systems Manager Boto3 Documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/ssm.html)