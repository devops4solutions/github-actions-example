
## Github Actions Workflow 
- What is Github Actions
- How to configure Github workflow in your repo ?
- Run workflow on existing github hosted runners.

## Upload on S3 bucket ( aws-s3-upload.yaml)

In this workflow, we will cover below points
  1. Run when push on s3 folder
  2. use a build matrix if you want your workflow to run tests across multiple combinations of operating systems, platforms, and languages. 
  3. Use of matrix to run the workflow for different environments or even you can set different variables as per the environment if needed
  4. Connect to AWS account using Github secrets
  5. Use of `workflow-dispatch` to trigger the workflow manually
  6. Use of `max-parallel` to run the job for both the environments in parallel
  7. Set working directory to any of your subfolder


## Deploy only when oush to main branch (deploy.yaml)

In this workflow, we will see how to configure a particular step to run only when there is a push to main branch



## References
1. https://docs.github.com/en/actions/learn-github-actions/managing-complex-workflows
2. https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions
