Stop a Run using a cron job that has been running for X hours

# Overview
This action performs the following:
- Install python 3.8
- Run pip install lightning-grid
- Run grid login --username xxx -key xxx
- Pauses a session based on what you define as GitHub's SESSION_NAME secret

# Usage
To stop a Run in cron fashion. Below is stop-run.yml. 

```
jobs:
  gridai-actions:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: gridai-actions/gridai-login@v0
        with:
          gridai-username: ${{ secrets.GRIDAI_USERNAME }} 
          gridai-key: ${{ secrets.GRIDAI_KEY }}
      - run: |
          grid session pause ${{ secrets.SESSION_NAME }} 
```


## Required
The repository requires the below secrets. See the offical [documentation](https://github.com/Azure/actions-workflow-samples/blob/master/assets/create-secrets-for-GitHub-workflows.md) for a walkthrough on creating secrets for GitHub workflows.
- GRIDAI_KEY 
- GRIDAI_USERNAME 
- RUN_NAME (has to be short)
- RUN_DURATION


