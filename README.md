# Twilio Studio GitOps

This repository demonstrates a GitOps approach for managing [Twilio Studio](https://www.twilio.com/studio) flows. Flow definitions are stored as JSON under the `.studio/` directory and can be promoted between environments via a GitHub Actions workflow.

## Pre-requisites

- Twilio Account SID and Auth Token stored as `TWILIO_ACCOUNT_SID` and `TWILIO_AUTH_TOKEN` secrets in the repository.
- GitHub Actions enabled on the repository.
- `curl` and `jq` available (the workflow installs `jq` automatically).

## How to Setup

1. Clone this repository.
2. For each Studio flow, create a folder under `.studio/<flow-name>/` and commit a `promoted-definition.json` file containing the flow's definition.
3. In GitHub, add the required Twilio credentials as repository secrets.
4. Customize `.github/workflows/promote-studio-flow.yml` if needed.

## How to Use

1. From the **Actions** tab in GitHub, run the **Promote Twilio Studio Flow** workflow.
2. Provide the inputs: `flow_name`, `from_env`, `to_env`, `from_flow_sid`, `to_flow_sid`, and `commit_message`.
3. The workflow fetches the flow definition from the source environment, rewrites environment-specific URLs, commits the promoted definition, and publishes it to the target environment.

