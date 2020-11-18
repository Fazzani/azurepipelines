# Azure pipelines templates

Azure pipelines reusable templates repository

## Getting Started

### Templates list

- [Basic .net core pipeline](./az-dotnet-core.yml)
  - Restore
  - Build
  - Test
  - Code coverage
- [.net core build && sonar pipeline](./az-dotnet-core-sonar.yml)
  - Basic .net core build steps
  - Sonar analyser
- [Basic React pipeline](./front-react-tpl.yml)
  - Build
  - Test
  - Lint
  - Code coverage
- [React build && sonar pipeline](./front-react-sonar-tpl.yml)
  - Basic React build steps
  - Sonar analyser

### Example

```yml
name: $(SourceBranchName)_$(date:yyyyMMdd)$(rev:.r)

resources:
  repositories:
    - repository: AzPipelinesTpl
      type: 'git'
      name: 'Up.France.ODI.AzurePipelines'

parameters:
- name: sonar_enabled
  displayName: Sonar enabled
  type: boolean
  default: true

pool:
  vmImage: $(imageName)

pr:
  branches:
    include:
    - master
    - develop
    - refs/tags/v*
  paths:
    exclude:
    - docs/*
    - README.md

trigger:
  branches:
    include:
    - master
    - develop
    - refs/tags/v*
  paths:
    exclude:
    - docs/*
    - readme.md

variables:
  buildConfiguration: 'Release'
  DOTNET_SKIP_FIRST_TIME_EXPERIENCE: true
  SONAR_PROJECT_KEY: 'sonar_key'
  SONAR_PROJECT_NAME: 'sonar_name'
  SONAR_ORG: up-france

strategy:
  matrix:
    linux:
     imageName: 'ubuntu-16.04'
    windows:
      imageName: 'windows-2019'
steps:
- template: az-dotnet-core-sonar.yml@AzPipelinesTpl
  parameters:
    sonar_project_org: $(SONAR_ORG)
    sonar_project_key: $(SONAR_PROJECT_KEY)
    sonar_project_name: $(SONAR_PROJECT_NAME)
    buildConfiguration: $(buildConfiguration)
    sonar_connection_service: sonarcloud
```

## TODO

- [x] AZ pipelines for front projects
- [ ] AZ pipeline for nuget packages

## Documentation

- [Azure template types & usage](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/templates?view=azure-devops)