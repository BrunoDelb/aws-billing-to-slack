# AWS Billing to Slack

![image](https://user-images.githubusercontent.com/261584/66362145-3903a200-e947-11e9-91bd-6e40e5919ac4.png)

Sends daily breakdowns of AWS costs to a Slack channel.

## Set up

Get the code:

```
git clone https://github.com/BrunoDelb/aws-billing-to-slack.git
cd aws-billing-to-slack
```

Customize the `credentials` file to set your AWS keys.

Create a new Incoming Webhook in Slack: `https://www.slack.com/apps/new/A0F7XDUAZ`.

You can an URL like `https://hooks.slack.com/services/XXXXXXXXXX`.

To build and deploy the Lambda Function:

```
docker run --rm -v $PWD/credentials:/root/.aws/credentials:ro -v $PWD/aws-billing-to-slack:/app aws-cli serverless deploy --slack_url="https://hooks.slack.com/services/XXXXXXXXXX"
```

To support cache, you can add `-v $PWD/cache:/root/.cache/pip`.

## Usage

Now you can schedule the call or call it manually:

```
docker run --rm -v $PWD/credentials:/root/.aws/credentials:ro -v $PWD/aws-billing-to-slack:/app aws-cli serverless invoke --function report_cost --slack_url="https://hooks.slack.com/services/XXXXXXXXX`X"
```

## Support for AWS Credits

If you have AWS credits on your account and want to see them taken into account on this report, head to [your billing dashboard](https://console.aws.amazon.com/billing/home?#/credits) and note down the "Expiration Date", "Amount Remaining", and the "as of" date towards the bottom of the page. Add all three of these items to the command line when executing the `deploy` or `invoke`:

    ```
    serverless deploy \
        --slack_url="https://hooks.slack.com/services/xxx/yyy/zzzz" \
        --credits_expire_date="mm/dd/yyyy" \
        --credits_remaining_date="mm/dd/yyyy" \
        --credits_remaining="xxx.xx"
    ```
