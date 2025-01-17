# Git sync

Some of the process for setting up codestar connections and repository links is not available in the AWS Console so please use the CLI where possible to validate.

https://govukverify.atlassian.net/wiki/spaces/PLAT/pages/4374954111/Setting+up+GitSync#2.-Configure-Your-Deployment-to-use-GitSync 

https://docs.aws.amazon.com/cli/latest/reference/codestar-connections/ 

## LZA

Landing zone accelerator custom templates are responsible for deploying an unconfirmed codestar connection and a code pipeline and build jobs.

## Init: Empty Stacks

You need to create an empty stack for gitsync to sync changes too. Follow the commands below making sure that the stack names are the same as in step 1 and 2 gitsync config.


## Step 1: Core infrastructure + eu-west-2 gitsync config

Deploy step-1 template in eu-west-2 which includes all the gitsync configuration, repo links and IAM roles and s3 buckets needed for FMS.

```bash
  ./step-1/deploy.sh Gitsync-core *ENVIRONMENT* GDS-GitHub-Connection main
```

## Step 2: Deploying slack notification event rules in both regions
Use the ```deploy.sh``` script in ```Step-2/notification-rules``` to iteratively deploy event rule resources to multiple regions.

```bash
  ./Step-2/deploy.sh GitSyncNotifications eu-west-2
```

### Step 3 Deploying slack integration
Use the ```deploy.sh``` script in ```Step-4/slack-integration``` to deploy slack integration resources.

```bash
  ./Step-3/deploy.sh GitSyncNotifications *SLACKCHANNELID* *SLACKWORKSPACEID*
```

## Parameters

Environment

    The deployment environment.

SLACKCHANNELID

    The Slack channel ID of the destination for messages.

SLACKWORKSPACEID

    The Slack workspace ID for the organization.

# Sandbox tests

The code pipleine is tested in the account for infrastructure/govenance/ environment which deploys a gitsync agains 4 stacks that deploy s3 buckets.

Everything else in the deployment spec as the same as it would be for a deployment into prod.