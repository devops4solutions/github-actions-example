# github-actions-example

# Upload on S3 bucket ( aws-s3-upload.yaml)
In this workflow, we will cover below points
  1. Run when push on s3 folder
  2. Run on multiple runners with different environment. You might have different runners for different environment or in different AWS accounts
  3. Use of matrix to run the workflow for different environments or even you can set different variables as per the environment if needed
  4. Connect to AWS account using Github secrets
  5. Use of `workflow-dispatch` to trigger the workflow manually
  6.  Use of `max-parallel` to run the job for both the environments in parallel
  7.  Set working directory to any of your subfolder
