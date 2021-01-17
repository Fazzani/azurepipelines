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
name: 1.0.$(BuildID)$(rev:.r)

resources:
  repositories:
    - repository: AzPipelinesTpl
      type: 'github'
      name: 'Fazzani/azurepipelines'
      endpoint: 'GitHub'
      #endpoint: 'Github-fazzani-pat'

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

strategy:
  matrix:
    linux:
     imageName: 'ubuntu-16.04'
     runtime: linux-x64
#    windows:
#      imageName: 'windows-2019'
steps:
- template: ci/az-dotnet-core.yml@AzPipelinesTpl
  parameters:
    buildConfiguration: $(buildConfiguration)

- task: DotNetCoreCLI@2
  displayName: '.NET publish $(buildConfiguration)'
  inputs:
    projects: src/ReleaseNotes/
    command: publish
    arguments: '--no-restore --no-build -c $(buildConfiguration) -r $(runtime) /p:PublishSingleFile=true /p:PublishTrimmed=true'

```

## Documentation

- [Azure template types & usage](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/templates?view=azure-devops)
