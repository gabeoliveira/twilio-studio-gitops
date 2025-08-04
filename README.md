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
2. Provide the inputs: `flow_name`, `from_env`, `to_env`, `from_flow_sid`, `to_flow_sid`, `commit_message`, and `replace_function_urls` (defaults to `true`).
3. If `replace_function_urls` is enabled, the workflow rewrites environment-specific Function URLs, commits the promoted definition, and publishes it to the target environment; otherwise it promotes the definition as-is.

## Advanced Usage

### Scheduled Promotions

#### How it works

The `.github/workflows/scheduled-promotion.yml` workflow runs on a cron schedule (weekdays at 6:00 AM UTC) and automatically promotes flows between environments. It reads the list of flows from `.studio/flows-config.json`, fetches the latest definition from the source environment, optionally rewrites environment-specific Function URLs, publishes the definition to the target environment, and commits the promoted definition back to the repository.

#### How to set it up

1. Ensure `.studio/flows-config.json` exists and includes an array of flow configurations with `flow_name`, `from_env`, `to_env`, `from_sid`, `to_sid`, and `replace_function_urls` fields.
2. Customize the schedule in `.github/workflows/scheduled-promotion.yml` if the default weekday 6:00 AM UTC run time does not fit your needs.
3. Confirm Twilio credentials (`TWILIO_ACCOUNT_SID` and `TWILIO_AUTH_TOKEN`) are available as repository secrets.

#### How to use it

Once configured, the Scheduled Promotion workflow runs automatically at the specified time and promotes any flows with changes. Monitor the workflow under the **Actions** tab to see promotion results.

