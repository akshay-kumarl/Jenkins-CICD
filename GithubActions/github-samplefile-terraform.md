```
name: Terraform Workflow

# Trigger the workflow on push or pull requests to the main branch
on:
  push:
    branches:
      - mainbranch
 
# Define the jobs to be executed
jobs:
  terraform:
    runs-on: ubuntu-latest
    environment: sample_env

    # env:  here directly can be written for job variables
      # aws-region: 'us-east-1'
      # TF_VERSION: '1.4.0'
      
    # Steps for the workflow
    steps:
    # Checkout the repository
    - name: Checkout repository
      uses: actions/checkout@v2

    # Configure AWS Credentials (if you're deploying on AWS)
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ secrets.AWS_REGION }}  # Specify your AWS region

    # Initialize Terraform (Download modules, plugins, etc.)
    - name: Terraform Init
      run: terraform init
      working-directory: infra/terraform/

    # Run Terraform Plan to see changes
    - name: Terraform Plan
      run: terraform plan
      working-directory: infra/terraform/
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        AWS-REGION: ${{ secrets.AWS_REGION }} 
        # AWS-REGION: ${{ env.aws-region }}

    # Apply changes (only for push events, not for pull requests)
    # - name: Terraform Apply
    #   # if: github.event_name == 'push'  # Run only on push
    #   run: terraform apply -auto-approve
    #   env:
    #     AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
    #     AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}

```
