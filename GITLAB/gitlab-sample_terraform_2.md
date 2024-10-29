sample terraform file to run via gitlab with runner

```
stages:
  - install
  - init
  - plan
  # - apply

# variables:
#   TERRAFORM_VERSION: "1.9.1"

tool_install:
  stage: install
  script: 
    - apt-get update -y && apt-get install -y unzip
    - curl "https://releases.hashicorp.com/terraform/1.9.1/terraform_1.9.1_linux_amd64.zip" -o terraform.zip
    - unzip terraform.zip 
    - mv terraform /usr/local/bin/
    - terraform --version
    # - cd ./infra/terraform  
    # - terraform init
    # - terraform plan -out=tfplan


terraform_init:
  stage: init
  script:
    - cd ./infra/terraform  
    - terraform init
  only:
    - mainbranch  # or specify the branch you want to run this on
  dependencies:
    - tool_install

terraform_plan:
  stage: plan
  script:
    - cd ./infra/terraform    
    - terraform plan -out=tfplan
  artifacts:
    paths:
      - tfplan
  only:
    - mainbranch
  dependencies:
    - tool_install     

# terraform_apply:
#   stage: apply
#   script:
#     - terraform apply -auto-approve tfplan
#   only:
#     - main
#   when: manual
#   dependencies:
#     - terraform_plan

```
