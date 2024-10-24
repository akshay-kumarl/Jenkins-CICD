
### Root Directory

```
/ (root)
│
├── .gitlab-ci.yml        # GitLab CI/CD configuration file
├── README.md             # Project documentation or introduction
├── LICENSE               # License file
├── src/                  # Source code directory (commonly used for app or service code)
├── tests/                # Tests directory (unit, integration, etc.)
├── config/               # Configuration files or settings for the project
├── scripts/              # Utility scripts or helper scripts
├── docker/               # Docker files (e.g., Dockerfiles, docker-compose)
└── docs/                 # Additional project documentation (if needed)
```


### General Structure of .gitlab-ci.yml

```
# Define stages (the order of execution)
stages:
  - stage1
  - stage2
  - stage3

# Define global variables (optional)
variables:
  GLOBAL_VAR: "some_value"

# Define before_script (optional): Executes commands before any job runs
before_script:
  - echo "Running before script"

# Job 1 (runs in stage1)
job_name_1:
  stage: stage1
  script:
    - echo "This is Job 1 in stage1"
  tags:
    - runner_tag  # For specific runners (optional)
  only:
    - branches
  except:
    - tags  # Jobs will not run on tags (optional)
  artifacts:
    paths:
      - path/to/artifact  # Store artifacts for later stages (optional)

# Job 2 (runs in stage2)
job_name_2:
  stage: stage2
  script:
    - echo "This is Job 2 in stage2"
  dependencies:
    - job_name_1  # Defines that this job depends on the artifacts of job_name_1
  allow_failure: true  # The pipeline will continue even if this job fails

# Job 3 (runs in stage3)
job_name_3:
  stage: stage3
  script:
    - echo "This is Job 3 in stage3"
  only:
    - master  # Will only run on the master branch

# After_script (optional): Executes commands after any job finishes
after_script:
  - echo "Running after script"

```
