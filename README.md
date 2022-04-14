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
name: 'Grid.ai Stop Run'
on:
   schedule:
     # Runs "every 30 mins" (see https://crontab.guru)
     - cron: '*/30 * * * *'
jobs:
  gridai-actions:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: gridai-actions/gridai-login@v0
        with:
          gridai-username: ${{ secrets.GRIDAI_USERNAME }} 
          gridai-key: ${{ secrets.GRIDAI_KEY }}   
      - name: get-run-status
        run: |
          export COLUMNS=512
          # run name is truncated within github action due to screen size.  make it short
          grid status
          export RUN_STATUS=$(grid status ${{ secrets.RUN_NAME }} |  awk '{print $2 "," $6}' | grep ${{ secrets.RUN_NAME }} | cut -d ',' -f 2 )
          echo "RUN_STATUS $RUN_STATUS"
          export RUN_HOURS=$(grid status ${{ secrets.RUN_NAME }} |  awk '{print $2 "," $10}' | grep ${{ secrets.RUN_NAME }} | cut -d ',' -f 2 | cut -d ':' -f 1 | cut -d '-' -f 2 )
          echo "RunHOURS1: $RUN_HOURS"
          if [ "$RUN_STATUS" != "running" ]
          then
             echo "Run is not running"
          elif [ "$RUN_HOURS" -gt "${{ secrets.RUN_DURATION }}" ]
          then
            echo "Run duration has exceeded crossed threshold: ${RUN_DURATION} Run is being Stopped"
            grid stop run ${{ secrets.RUN_NAME }}
          fi
        shell: bash

```


## Required
The repository requires the below secrets. See the offical [documentation](https://github.com/Azure/actions-workflow-samples/blob/master/assets/create-secrets-for-GitHub-workflows.md) for a walkthrough on creating secrets for GitHub workflows.
- GRIDAI_KEY 
- GRIDAI_USERNAME 
- RUN_NAME (Make sure there is no space after the Run name)
- RUN_DURATION (For 2 hours enter 02)


