# Azure_Docker_ArmPush
Use this task to run a PowerShell script within an Azure environment. The Azure context is authenticated with the provided Azure Resource Manager service connection &amp; then pushes the docker images.

## Parameters:

The pipeline requires the following parameters to be defined:


| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| azureTaskType | Type of Azure Task | string | 'powershell' | 'powershell' / 'cli' | Optional | Required when **serviceConnectionType** is 'arm'| 
| serviceConnection | Azure Service Connection for acr registry or azure resource manager in your Azure pipeline | string | | | Required | This helps the module to authenticate with registry or azure cli |
| imageACR | Azure Container Registry Name | string | | | Optional | Required when serviceConnectionType is 'arm' |
| imageRepository | acr docker repostiroy | string | | | Required | docker registry imagename to be created |
| dockerfilePath  | location of Dockerfile | string | | | Optional | Required when buid step |
| tags | Version or tag for image | string |  | | Required | when multiple tags pass in following way: **"1.2.3,latest"** |
| buildContext  | the path to constitute everything for Docker Image | string | '.' | | Optional | Optionally used with build step |
| dockerBuildArgs | arguments for build step | string | | | Optional |--build-arg var.value=1 |
--------------------------------------------------------------------------------------------------------------------------------------------------

These parameters provide multiple use case options for the template, enable/disable flags for the utilization of different templates as per the requirements.


## Use Cases

You can directly call a particular template as per the requirement. for example: 

  ```yaml
  # azure-pipeline.yml
  resources:
  repositories:
    - repository: Template
      type: github
      name: your_username/Azure_Docker_ArmPush
      ref: <respective branch name>
      endpoint: 'githubServiceConnectioNname'

  steps:
  # passing the parameters
  - template: Azure_Docker_ArmPush.yaml
    parameters:
          serviceConnection: ${{parameters.serviceConnection}}
          imageACR: ${{ parameters.imageACR }}
          imageRepository: '${{parameters.imageRepository}}'
          tags: ${{parameters.tags}}
          azureTaskType: ${{parameters.azureTaskType}}  
          dockerPushArgs: ${{parameters.dockerPushArgs}}
        
  
Make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.

  ```
