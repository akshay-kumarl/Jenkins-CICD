## Github Actions Notes and sample files

### Folder structure
```
.github/ 
└── workflows/ 
    └── my-workflow.yml
```

---


### General Structure of a GitHub Actions YAML file

```
# Name of the workflow (optional)
name: Workflow Name

# Define when the workflow should run (on events)
on:
  push:          # Trigger the workflow on push
    branches:
      - main     # Specify branches (can also be tags)
  pull_request:  # Trigger on pull requests (optional)
  schedule:      # Define a cron schedule for the workflow (optional)
    - cron: '0 0 * * *'  # Runs daily at midnight UTC

# Define environment variables (optional)
env:
  GLOBAL_VAR: some_value

# Define all the jobs
jobs:
  # Job 1
  job_name_1:
    runs-on: ubuntu-latest    # Define the runner (e.g., Ubuntu, macOS, Windows)
    steps:
      - name: Checkout code   # Step 1: Check out the repository code
        uses: actions/checkout@v3

      - name: Run a script     # Step 2: Run a shell command
        run: echo "Running job 1"

  # Job 2 (dependent on job 1)
  job_name_2:
    runs-on: ubuntu-latest
    needs: job_name_1         # Job 2 depends on the successful completion of Job 1
    steps:
      - name: Run another script
        run: echo "Running job 2 after job 1"

  # Job 3 (run on a specific matrix of configurations)
  job_name_3:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [12, 14, 16]  # Run this job for multiple Node.js versions
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}

      - name: Run tests
        run: npm test

```




If you have any questions, you can refer them to ChatGPT for assistance. type as 'general structure of  GitHub actions'

---
### Event:

Events are specific activities that trigger workflows. Examples of events include: <br/>
> **push:** When code is pushed to the repository.<br/>
> **pull_request:** When a pull request is opened or updated.<br/>
> **schedule:** A scheduled time event (e.g., cron jobs).<br/>
> **release:** When a release is published.
