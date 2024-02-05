# SecureCodingPipelineDecorator

The idea here is to provide the basis of a Pipeline Decorator that performs some checks Pre and Post job executions.

## Requirements

- Execute Pre and Post checks on Jobs - these checks are different actions / pipelines
- Execute only for specific Projects
- Don't execute a task if it has already been executed in the original pipeline
- Only execute on "main" branch
- Skip execution if variable "secureCoding.SkipChecks"  is set to "true"

## The solution

- Create 2 different Pipeline Decorators on the following folders: 
    - Pre
    - Post

## Test pipelines

- Under for TestPipelines
    - 11_SecureCodingPipelineSimple.yaml - expected is that all Pre and Post tasks are executed
    - 12_SecureCodingPipelineWithNetCore.yaml - expected is Task DotNet install from Post is not executed

## Generating / Updating the vsix file

1) Create release: tfx extension create
    1.1) Create updates: tfx extension create --rev-version
2) Upload to https://marketplace.visualstudio.com/manage