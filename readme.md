# Masterdeploy

## Introduction

This template deploys other templates

## Security Controls

The following security controls can be met through configuration of this template:

* None documented

## Dependancies

* None

## Parameter format

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "deploymentSubArray": {
            "value": [
                {
                    "name": "resourcegroups",
                    "location": "canadacentral",
                    "templateLink": "https://raw.githubusercontent.com/canada-ca-azure-templates/resourcegroups/20190514/template/azuredeploysub.json",
                    "parametersFile": "https://raw.githubusercontent.com/canada-ca-azure-templates/keyvaults/dev/test/parameters/dependancy-resourcegroups-canadacentral.parameters.json"
                },
                {
                    "name": "validate",
                    "location": "canadacentral",
                    "templateLink": "https://raw.githubusercontent.com/canada-ca-azure-templates/keyvaults/dev/template/azuredeploysub.json",
                    "parametersFile": "https://raw.githubusercontent.com/canada-ca-azure-templates/keyvaults/dev/test/parameters/validate.parameters.json",
                    "dependsOn": [
                        "resourcegroups"
                    ]
                }
            ]
        }
    }
}
```

## Parameter Values

### Main Template

| Name               | Type   | Required | Value                                                               |
| ------------------ | ------ | -------- | ------------------------------------------------------------------- |
| parametersSasToken | string | No       | SAS Token received to authorize access to protected parameters uris |
| templateSasToken   | string | No       | SAS Token received to authorize access to protected templates uris  |
| baseParametersURL  | string | No       | Parameters URL Prefix                                               |
| deploymentSubArray | array  | Yes      | Array of [Deployments Objects](#deployments-object)                 |

#### Deployments Object

| Name           | Type    | Required | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| -------------- | ------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| name           | string  | Yes      | Name of deployment. Must be unique in the local parameter file                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| location       | string  | Yes      | Location where the resource is deployed. - canadacentral or canadaeast                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| templateLink   | string  | Yes      | URL for the template to use for the deployment                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| parametersFile | string  | Yes      | Url (or the missing part of the url if using the baseParameterURL parametes above) for the parameters file to be used with the template                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| private        | boolean | No       | This optional parameter allow you to specify the use of a private Template Library Token if you need to deploy from a non public library. This only support a single private Template library as only one private Template Token can be passed to the masterdeploy templates at the moment. You will also need to pass this ARM deployment parameter under the name of "-templateSasToken <value>" in your powershell script. The value of the private parameter can be set to true of false as shown below. A value of false will be equivalent to not having the private option in the parameter... meaning it is consider as if you do not deploy from a private Template Library |
| dependsOn      | array   | No       | Array of Deployments Object name string that need to be completed before this deployment object is deployed. E.g: [ "resourcegroup", "vnet" ]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |

## History

| Date     | Release                                                                             | Change                                              |
| -------- | ----------------------------------------------------------------------------------- | --------------------------------------------------- |
| 20190317 |                                                                                     | 1st release                                         |
| 20190309 |                                                                                     | Improving how dependsOn is handles in the templates |
| 20190410 |                                                                                     | Add support for optional private library deployment |
| 20190514 | [20190514](https://github.com/canada-ca-azure-templates/masterdeploy/tree/20190514) | Move to new github structure                        |