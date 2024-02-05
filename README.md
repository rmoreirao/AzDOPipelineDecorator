# Sample Pipeline Decorator

## Steps to Create and Debug a Pipeline Decorator

### Step 1: create the manifest (.json) and the decorator yaml pipeline

| MyFirstPipelineDecorator
| -- vss-extension.json
| -- myfirstpipeline.yml

### Step 2: Create the Extension
1) Install npm: npm install -g tfx-cli
2) Create release .vsix file: tfx extension create
3) Create updates: tfx extension create --rev-version

### Step 3: Updload the Extension to Market Place
1) Access: https://marketplace.visualstudio.com/manage
2) Create the publisher - *publisher account is important, as this account would also need to be an administrator of your Azure DevOps organization*
3) Upload your extension:
    3.1) Upload the *.vsix file
    3.2) ID contained in its manifest (vss-extension.json) matches exactly the name of the publisher account you just created. No spaces in the name.
    3.3) Extension must be "Private"
4) Check created Extension
    5.1) Click the "..." on the Extension and "View"
5) Make it available to your organization
    4.1) Click the "..." on the Extension and "Share / Unshare"
    4.2) Add Organizations you want to enable it
6) Install the extension
    6.1) Got to your Organization Settings -> Extensions
    6.2) Click on "Shared" tab
    6.3) Click on the Extension and install it

### Step 4: How to Execute and Debug Pipeline decorator
1) Import Azure DevOps pipelines from folder "MyFirstPipelineDecoratorTestPipelines" and execute them - Pipeline Decorators will be added to your Pipeline executions
    1.1) These will apply to all Pipelines of your Organization

2) To debug your Pipeline Decorator add the variable "system.debugContext" = "true"

## Sample Pipeline Decorator: MyFirstPipelineDecorator
### Specific Requirements:
- Only execute for Pipelines under "PipelineDecorator" project
- Only execute on Main branch
- Skip for Specific Projects
- Skip when specific Pipeline Variable is set to True

### Test Pipelines - Manual execution
1) Execute in a normal Pipeline (No Jobs): should execute - 01_simpletask.yaml
2) Execute in a normal Pipeline on another branch - not main branch: should not execute - 02_SimpleTaskAllBranches.yaml
3) Execute when Pipeline Fails (With Jobs): should not execute - 03_FailPipeline.yaml
4) Execute when Pipeline Success With Jobs: should execute - 04_PipelineWithJobs.yaml
5) Execute when Pipeline Success With Stages: should execute - 05_PipelineWithJobsAndStages.yaml
6) Execute when Specific Variable is set: should execute - 06_SimpleTaskMainBranchWithVariable.yaml
7) Debug pipeline decorator: should execute - 06_SimpleTaskMainBranchWithVariable.yaml

## Samples

### Filter based on specific variable
```
steps:
- ${{ if ne(variables['skipDecoratorInjection'], 'true')) }}:
  - task: 
```
*** List with all predefined variables: https://learn.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml

### Filter by Project
```
steps:
- ${{ if eq(variables['System.TeamProject'], 'ProjectA')  }}:
  - task: anytask
```
### Filter by Operating System
```
- task: CmdLine@2
  displayName: '(Injected) Task test: OS is Windows'
  condition: eq(variables['Agent.OS'], 'Windows_NT')
  inputs:
    script: |
        echo %AGENT_OS% 

- task: CmdLine@2
  displayName: '(Injected) Task test: OS is Linux'
  condition: eq(variables['Agent.OS'], 'Linux')
  inputs:
    script: |
        echo $AGENT_OS 

```
### Skip if task is already present
```
steps:
- ${{ if containsValue(job.steps.*.task.id, 'EA576CD4-C61F-48F8-97E7-A3CB07B90A6F') }}::
  - task: 
```
*** Task ID can be found on task.json of the task - here all default tasks: https://github.com/microsoft/azure-pipelines-tasks/tree/master/Tasks

### Check Stage name
```
(contains(variables['System.StageDisplayName'],'PRD'),contains(variables['System.StageDisplayName'],'Prod'))
```

# Resources for Creating this repo:
https://learn.microsoft.com/en-us/azure/devops/extend/develop/add-pipeline-decorator?view=azure-devops

https://learn.microsoft.com/en-us/azure/devops/extend/develop/pipeline-decorator-context?view=azure-devops

https://learn.microsoft.com/en-us/azure/devops/pipelines/process/expressions?view=azure-devops

Sample with Multiline conditions: https://stackoverflow.com/questions/77004197/azure-devops-multiline-yaml-key-for-if-condition

https://github.com/n3wt0n/AzurePipelinesDecoratorSamples/

https://github.com/lgmorand/azure-devops-pipeline-decorators and https://lgmorand.github.io/blog/azure-devops-pipeline-decorator-part1 

https://github.com/JLee794-Sandbox/ADO-Decorators-PoC

