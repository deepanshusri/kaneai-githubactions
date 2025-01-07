# Parameterized Github Actions Setup for KaneAI

This GitHub Actions workflow allows you to run a parameterized cURL command to trigger LambdaTest's HyperExecute API with dynamic input values for test_run_id, concurrency, title, and region.

Prerequisites
GitHub Repository Access: Ensure you have push access to the repository where the workflow will be set up.
GitHub Secrets: Add the Base64-encoded authorization token as a secret in your repository:
Go to your repository.
Navigate to Settings > Secrets and variables > Actions > New repository secret.
Add a new secret with the name AUTH_HEADER and set its value to your Base64-encoded authorization token.
Setting Up the Workflow
Create a directory named .github/workflows in the root of your repository if it doesn’t already exist.

Inside the .github/workflows directory, create a new file called run-parameterized-curl.yml and paste the following content:

yaml
Copy code
name: Parameterized cURL Command

on:
  workflow_dispatch:
    inputs:
      test_run_id:
        description: 'The Test Run ID'
        required: true
        default: 'YOUR_TEST_RUN_ID'
      concurrency:
        description: 'Concurrency Level'
        required: false
        default: 1
      title:
        description: 'Unique Build Name'
        required: false
        default: 'UNIQUE_BUILD_NAME'
      region:
        description: 'Desired Region'
        required: true
        default: 'eastus'

jobs:
  run-curl:
    runs-on: ubuntu-latest
    steps:
      - name: Run Parameterized cURL Command
        run: |
          curl --location 'https://test-manager-api.lambdatest.com/api/atm/v1/hyperexecute' \
          --header 'Content-Type: application/json' \
          --header "Authorization: ${{ secrets.AUTH_HEADER }}" \
          --data "{
              \"test_run_id\": \"${{ github.event.inputs.test_run_id }}\",
              \"concurrency\": ${{ github.event.inputs.concurrency }},
              \"title\": \"${{ github.event.inputs.title }}\",
              \"region\": \"${{ github.event.inputs.region }}\"
          }"
Commit and push the file to your repository.

How to Use
Go to the Actions tab in your GitHub repository.
Select the Parameterized cURL Command workflow.
Click Run workflow.
Fill in the input fields:
Test Run ID: Enter the test run ID (required).
Concurrency: Set the concurrency level (default is 1).
Title: Provide a unique build name (default is UNIQUE_BUILD_NAME).
Region: Specify the desired region (eastus, centralindia, etc.).
Click Run workflow to execute the cURL command.
Notes
The AUTH_HEADER secret should be securely stored in GitHub Secrets to prevent unauthorized access.
You can modify the default values in the workflow_dispatch inputs as per your requirements.
