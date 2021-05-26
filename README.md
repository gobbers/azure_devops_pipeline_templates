# Gobber Azure DevOps templates

This repository contains common pipeline templates.

## Usage
Create a `.azure-pipelines.yaml` file in your repository with the following content:

```yaml
pool:
  name: Enlight
  demands:
    - sh
    - ADO_AGENT_PREFIX -equals linux-build-agent-ci
    
resources:
  repositories:
  - repository: templates
    type: github
    name: gobbers/azure_devops_pipeline_templates
    endpoint: SKF

extends:
  template: templates/pipeline-template.yaml@templates
  parameters:
    tfVersion: "0.15.4"
    buildJobs:
    - job: job1
      displayName: build 
      steps:
      - script: echo build

    - job: job2
      displayName: test
      steps:
      - script: echo test

```

The `buildJobs` parameter is a standard sequence of [Jobs](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema#job) that are needed to build the componentes needed.

The [pipeline-template.yaml](templates/pipeline-template.yaml) creates the default [stages](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema#job) and checkpoints that we want in the pipeline and handles [deployments](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema#job) to our different environments.

# Limitations
Currently the only supported deployment mode is using [Terraform](https://terraform.io) and the `buildJobs` must publish the terraform artifacts with the name `terraform`.
The terraform folder layout must be:
```
terraform
|- env
  |- prod
  |- sandbox
  |- staging
  |- test

```

See the [templates/terraform-deploy-template.yaml](templates/terraform-deploy-template.yaml).