# Parameterized CI Setup with Github Actions for KaneAI

This GitHub Actions workflow allows you to run a parameterized cURL command and showcase how KaneAI tests can be executed via CI setup from Github Action using with dynamic input values for test_run_id, concurrency, title, and region.

## Prerequisites

Before setting up the workflow, ensure the following:

1. **GitHub Repository Access**: You have push access to the repository.
2. **GitHub Secrets**: Add the Base64-encoded authorization token as a secret:
   - Go to your repository.
   - Navigate to **Settings > Secrets and variables > Actions > New repository secret**.
   - Add a new secret:
     - **Name**: `AUTH_HEADER`
     - **Value**: Your Base64-encoded authorization token.

## Setting Up the Workflow

Follow these steps to set up the workflow:

1. Create a directory named `.github/workflows` in the root of your repository (if it doesn’t exist).
2. Create a new file in the `.github/workflows` directory named `run-parameterized-curl.yml`.
3. Copy and paste the following content into the file:

   ```yaml
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
   
  4.Commit and push the file to your repository.


## How to Run the Workflow
1. Navigate to the Actions tab in your GitHub repository.
2. Select the Parameterized cURL Command workflow.
3. Click the Run workflow button.
4. Provide the following input values:
   - Test Run ID: Enter the test run ID (required).
   - Concurrency: Specify the concurrency level (default: 1).
   - Title: Provide a unique build name (default: UNIQUE_BUILD_NAME).
   - Region: Specify the desired region (e.g., eastus, centralindia).
   - Click Run workflow to execute the cURL command.


## Notes

- Keep the AUTH_HEADER secret secure to prevent unauthorized access.
- Modify the default input values in the workflow_dispatch section if needed.
- This workflow is designed to provide flexibility and ease of use for running HyperExecute API commands.
