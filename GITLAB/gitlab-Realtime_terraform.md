

```
default:
  tags:
    - Terraform
    - AWS

variables:
  TERRAFORM_DESTROY: "YES"
  TERRAFORM_APPLY: "NO"
  TERRAFORM_VERSION: "1.6.1"

stages:
  - terraform-version-check
  - terraform-init
  - terraform-fmt-validate
  - terraform-plan
  - terraform-apply
  - terraform-destroy

# Terraform Version Check Job
job-check-terraform-version:
  stage: terraform-version-check
  rules:
    - if: $TERRAFORM_DESTROY == "NO" && $TERRAFORM_APPLY == "YES"
  script:
    - pwd && ls -al
    - tfenv install 1.6.1 && tfenv use 1.6.1
    - terraform version

# Terraform Init Job for Developmentt
job-run-terraform-init-development:
  stage: terraform-init
  rules:
    - if: $TERRAFORM_DESTROY == "NO" && $TERRAFORM_APPLY == "YES"
  script:
    - cd development
    - terraform init

# Terraform Init Job for Production
job-run-terraform-init-production:
  stage: terraform-init
  rules:
    - if: $TERRAFORM_DESTROY == "NO" && $TERRAFORM_APPLY == "YES"
  script:
    - cd production
    - terraform init

# Terraform Format and Validate Job for Development
job-run-terraform-fmt-validate-development:
  stage: terraform-fmt-validate
  rules:
    - if: $TERRAFORM_DESTROY == "NO" && $TERRAFORM_APPLY == "YES"
  script:
    - cd development
    - terraform init
    - terraform fmt
    - terraform validate

# Terraform Format and Validate Job for Production
job-run-terraform-fmt-validate-production:
  stage: terraform-fmt-validate
  rules:
    - if: $TERRAFORM_DESTROY == "NO" && $TERRAFORM_APPLY == "YES"
  script:
    - cd production
    - terraform init
    - terraform fmt
    - terraform validate

# Terraform Plan Job for Development
job-run-terraform-plan-development:
  stage: terraform-plan
  rules:
    - if: $TERRAFORM_DESTROY == "NO" && $TERRAFORM_APPLY == "YES"
  script:
    - cd development
    - terraform init
    - terraform plan

# Terraform Plan Job for Production
job-run-terraform-plan-production:
  stage: terraform-plan
  rules:
    - if: $TERRAFORM_DESTROY == "NO" && $TERRAFORM_APPLY == "YES"
  script:
    - cd production
    - terraform init
    - terraform plan

# Terraform Apply Job for Development
job-run-terraform-apply-development:
  stage: terraform-apply
  needs: ["job-run-terraform-plan-development"]
  rules:
    - if: $TERRAFORM_DESTROY == "NO" && $TERRAFORM_APPLY == "YES"
  script:
    - cd development
    - terraform init
    - terraform apply --auto-approve

# Terraform Apply Job for Production
job-run-terraform-apply-production:
  stage: terraform-apply
  needs: ["job-run-terraform-plan-production"]
  rules:
    - if: $TERRAFORM_DESTROY == "NO" && $TERRAFORM_APPLY == "YES"
  script:
    - cd production
    - terraform init
    - terraform apply --auto-approve

# Terraform Destroy Job for Development
job-run-terraform-destroy-development:
  stage: terraform-destroy
  rules:
    - if: $TERRAFORM_DESTROY == "YES"
  script:
    - cd development
    - terraform init
    - terraform destroy --auto-approve

# Terraform Destroy Job for Production
job-run-terraform-destroy-production:
  stage: terraform-destroy
  rules:
    - if: $TERRAFORM_DESTROY == "YES"
  script:
    - cd production
    - terraform init
    - terraform destroy --auto-approve

```

