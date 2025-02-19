# Overall
This directory contains management plane service clients of Az.Aks module.

## Run Generation
In this directory, run AutoRest:
```
autorest --reset
autorest.cmd README.md --version=v2
```

### AutoRest Configuration
> see https://aka.ms/autorest
``` yaml
csharp: true
clear-output-folder: true
openapi-type: arm
azure-arm: true
license-header: MICROSOFT_MIT_NO_VERSION
payload-flattening-threshold: 1
```

###
``` yaml
input-file:
  - https://github.com/Azure/azure-rest-api-specs/blob/9d8a951af5d78e24d9d83592107f8d3c2cc417f5/specification/containerservice/resource-manager/Microsoft.ContainerService/aks/stable/2023-02-01/managedClusters.json

### There are 2 same "type" property with same x-ms-enum.name="ResourceIdentityType" defined in both managedClusters.json and its referenced types.json. 
### Rename the one in types.json to avoid autorest converting error.
### There are <> in description of orchestratorVersion & currentOrchestratorVersion & kubernetesVersion & currentKubernetesVersion, which will make dotnet build failed.
### Replace <> with () in these descriptions.
directive:
  - from: swagger-document
    where: $.definitions.Identity.properties.type["x-ms-enum"]
    transform: $.name = "ResourceIdentityTypeForCommonTypes"
  - from: swagger-document
    where: $.definitions.ManagedClusterAgentPoolProfileProperties.properties.orchestratorVersion
    transform: $["description"] = $["description"].replace(/</g, "(");
  - from: swagger-document
    where: $.definitions.ManagedClusterAgentPoolProfileProperties.properties.orchestratorVersion
    transform: $["description"] = $["description"].replace(/>/g, ")");
  - from: swagger-document
    where: $.definitions.ManagedClusterAgentPoolProfileProperties.properties.currentOrchestratorVersion
    transform: $["description"] = $["description"].replace(/</g, "(");
  - from: swagger-document
    where: $.definitions.ManagedClusterAgentPoolProfileProperties.properties.currentOrchestratorVersion
    transform: $["description"] = $["description"].replace(/>/g, ")");
  - from: swagger-document
    where: $.definitions.ManagedClusterProperties.properties.kubernetesVersion
    transform: $["description"] = $["description"].replace(/</g, "(");
  - from: swagger-document
    where: $.definitions.ManagedClusterProperties.properties.kubernetesVersion
    transform: $["description"] = $["description"].replace(/>/g, ")");
  - from: swagger-document
    where: $.definitions.ManagedClusterProperties.properties.currentKubernetesVersion
    transform: $["description"] = $["description"].replace(/</g, "(");
  - from: swagger-document
    where: $.definitions.ManagedClusterProperties.properties.currentKubernetesVersion
    transform: $["description"] = $["description"].replace(/>/g, ")");

output-folder: Generated
namespace: Microsoft.Azure.Management.ContainerService
```