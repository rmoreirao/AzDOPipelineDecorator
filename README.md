# Sample Pipeline Decorator

## Step 1: create the manifest and the decorator pipeline

| banner-decorator
| -- vss-extension.json
| -- banner-decorator.yml

## Step 2: Create the Extension
npm install -g tfx-cli
tfx extension create

## Step 3: Updload the Extension to Market Place
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

## How to Execute and Debug Pipeline decorator

## Specific Requirements:
- Only execute on Main branch
- Skip for Specific Projects
- Skip when specific Pipeline Variable is set to True

## Tests - Post
### 1) Execute in a normal Pipeline (No Jobs): should execute - 01_simpletask.yaml
### 2.1) Execute in a normal Pipeline (Job on Main Branch): should execute - 02_SimpleTaskAllBranches.yaml
### 2.2) Execute in a normal Pipeline (Job on Other Branch): should not execute - 02_SimpleTaskAllBranches.yaml
### 3) Execute when Pipeline Fails (With Jobs): should not execute - 03_FailPipeline.yaml
### 5) Execute when Pipeline Success With Jobs: should execute
### 6) Execute when Pipeline Success With Stages: should execute


# Resources:
https://learn.microsoft.com/en-us/azure/devops/extend/develop/add-pipeline-decorator?view=azure-devops
