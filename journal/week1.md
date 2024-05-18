# Week 1

## AWS account setup
1. Create new users as IAM users, give them admin access
2. Set access key in security credentials of users
3. Setup billing and budget for users

## Setting up gitpod workspace
1. Update .gitpod.yml
~~~text
tasks:
  - name: aws-cli
    env:
      AWS_CLI_AUTO_PROMPT: on-partial
    init: |
      cd /workspace
      curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
      unzip awscliv2.zip
      sudo ./aws/install
      cd $THEIA_WORKSPACE_ROOT
      
vscode:
  extensions:
    - 42Crunch.vscode-openapi
~~~

2. Set aws credentials in gitpod so that it remembers "aws sts get-caller-identity" if we relaunch the workspace
~~~text
Command line:
  gp env AWS_ACCESS_KEY_ID=<aws access key>
  gp env AWS_SECRET_ACCESS_KEY=<secret access key>
  gp env AWS_DEFAULT_REGION="eu-central-1"
~~~

## Setting up budget
~~~text
1. Create "budget.json" and "budget-notifications-with-subscriber.json" under "aws/json" folder (examples taken from https://docs.aws.amazon.com/cli/latest/reference/budgets/create-budget.html#examples)
2. Command line:
  aws budgets create-budget \
      --account-id 654654406147 \
      --budget file://aws/json/budget.json \
      --notifications-with-subscribers file://aws/json/budget-notifications-with-subscribers.json
~~~
## Setting up notifications
~~~text
Command line:
  aws sns create-topic \
      --name billing-alarm
  aws sns subscribe \
      --topic-arn arn:aws:sns:eu-central-1:654654406147:billing-alarm \
      --protocol email \
      --notification-endpoint svdp.mukherjee.lux@gmail.com
~~~

## Setting up alarm
~~~text
1. Create "alarm-cofig.json" under "aws/json" folder (examples taken from https://repost.aws/knowledge-center/cloudwatch-estimatedcharges-alarm)
2. Command line:
  aws cloudwatch put-metric-alarm --cli-input-json file://aws/json/alarm-config.json
~~~
