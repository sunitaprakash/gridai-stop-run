Stop a Run using a cron job that has been running for X hours

# Overview
This action performs the following:
- Install python 3.8
- Run pip install lightning-grid
- Run grid login --username xxx -key xxx
- Delete a Run that been running for X run of hours. Run name and Run Duration is based on what you define as GitHub's RUN_NAME and RUN_DURATION secret

# Usage
To stop a Run in cron fashion. Below is stop-run.yml. 

```

```


## Required
The repository requires the below secrets. See the offical [documentation](https://github.com/Azure/actions-workflow-samples/blob/master/assets/create-secrets-for-GitHub-workflows.md) for a walkthrough on creating secrets for GitHub workflows.
- GRIDAI_KEY 
- GRIDAI_USERNAME 
- RUN_NAME (run name is truncated within github action due to screen size.  make it short)
- RUN_DURATION


