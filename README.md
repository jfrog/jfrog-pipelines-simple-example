# jfrog-pipelines-simple-example

This is a simple, "hello world" level example that defines two pipelines:
- `my_fiest_pipeline contains 3 linear steps, triggered by a Github repository. The steps demonstrate simple tasks like reading information from the input resource and writing to and reading from run/pipeline state.  The last step in this pipeline also writes the Github commitSha that triggered the run to an output resource of type "PropertyBag".
- `my_second_pipeline` has one step which is triggered by the same PropertyBag resource that is updated by the first pipeline. This shows how you can define dependent pipelines through the  use  of resources.    

![alt text](https://github.com/jfrog/jfrog-pipelines-simple-example/blob/master/images/hello-world-pipeline-image.png "JFrog Pipelines Hello world")

Pre-requisites:

- JFrog Cloud account with Artifactory and Pipelines, or [JFrog Pipelines installed](https://www.jfrog.com/confluence/display/CICD/Installing+JFrog+Pipelines) (needs Artifactory 6.11 or above)
- User account created in Artifactory with deploy permissions to at least one binary repository  

Steps to run this pipeline:

- Fork this repository to your Github account
- Sign in to JFrog Platform
- Follow instructions to [add an integration](https://www.jfrog.com/confluence/display/CICD/Adding+Integrations) and create a [Github integration](https://www.jfrog.com/confluence/display/CICD/GitHub+Integration) to connect Github to Pipelines
- Edit the file `values.yml` in your fork of this repo and replace the following:
    - gitProvider should be set to the name of your Github integration created in the previous step
    - path should point to your fork of this repository
  Save and commit the file.  
- From the JFrog UI, [Add a pipeline source](https://www.jfrog.com/confluence/display/CICD/Adding+a+Pipeline+Source) and point it to the `jfrog-pipelines-hello-world.yml` in your fork of this repo. This adds your configuration to the platform and both pipelines are created based on your YAML. At this point, you can go to the **My Pipelines** page and see your pipelines.
- You can now commit to the repo to trigger your pipeline, or trigger it manually through the UI.

These pipelines demonstrate the following:

- Defining a [GitRepo resource](https://www.jfrog.com/confluence/display/CICD/GitRepo), which acts as the trigger for the pipeline. When this resource is created, the platform automatically adds webhooks to the source control repository with the specified configuration.
- Defining a [Bash step](https://www.jfrog.com/confluence/display/CICD/Bash), which lets you execute any commands you need for your automation.
- Defining `inputResources` and `inputSteps` to create a workflow
- Using environment variables (e.g. $res_someRepo_commitSha) to extract information from `inputResources`
- Using utility functions for state mgmt (e.g. add_run_variables first_stepid=$step_id) to exchange information across steps
- Connecting multiple pipelines through resources
