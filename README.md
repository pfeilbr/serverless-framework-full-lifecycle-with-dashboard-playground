
# serverless-framework-full-lifecycle-with-dashboard-playground

learn and understand how [Serverless Framework – Now, Full Lifecycle](https://serverless.com/blog/serverless-now-full-lifecycle/) works.  This allows serverless to instrument code, send cloudwatch logs to the serverless SaaS.  This allows serverless to provide the [Serverless Dashboard](https://serverless.com/framework/docs/dashboard/) features

---

**Running `serverless` from cli to create project**

![](https://www.evernote.com/l/AAGMIc4-FIJCSZXUtSiZZqr-jnavut4x_A0B/image.png)

The `service`, `app`, and `org` top-level properties added to `serverless.yml` enable serverless full lifecycle / dashboard

```yaml
service: serverless-with-dashboard-playground-01
app: serverless-with-dashboard-playground-01
org: pfeilbr
```

![](https://www.evernote.com/l/AAH9J_9FRfFGH7uiu2JonnyeBowUC_-cU0kB/image.png)

**Instrumented/wrapped Code Example**

On deploy, serverless instruments/wraps your code/handlers.  You don't see this locally in your codebase.  You only see on the deployment side in AWS.

![](https://www.evernote.com/l/AAHPyNZCdhlEwbjGnpVNd1Qr8IiEHLdfRB4B/image.png)

embedded/bundled serverless SDK
![](https://www.evernote.com/l/AAEWGJxVG6BCFY7nuiFMRRdiEeBOaC-ACjoB/image.png)


**`SERVERLESS_ENTERPRISE` wrapped log example**

serverless hooks logging to stdout and stderr via `serverlessSDK`.  This allows it to log structured JSON logs to cloudwatch logs with the prefix `SERVERLESS_ENTERPRISE`.  This logging is additional, the `console.log`s are logged independently.

![](https://www.evernote.com/l/AAGXImGSXy5ATKrP5-qDPHT3RMs7OfMMj9wB/image.png)

To see the log group subscription details

`aws logs describe-subscription-filters --log-group-name '/aws/lambda/serverless-with-dashboard-playground-01-dev-hello'`

```json
{
    "subscriptionFilters": [
        {
            "filterPattern": "?\"REPORT RequestId: \" ?\"SERVERLESS_ENTERPRISE\"",
            "filterName": "serverless-with-dashboard-playground-01-dev-CloudWatchLogsSubscriptionFilterHelloLogGroup-1SAKWRFMW5JHE",
            "creationTime": 1566318781436,
            "logGroupName": "/aws/lambda/serverless-with-dashboard-playground-01-dev-hello",
            "destinationArn": "arn:aws:logs:us-east-1:802587217904:destination:LGGXBmZw2Z47MmWq6b#VlGYyRJNfvVVgHf8y1#serverless-with-dashboard-playground-01#dev",
            "distribution": "ByLogStream"
        }
    ]
}
```

It sends logs to the serverless AWS account (`802587217904`).  The `destinationArn: arn:aws:logs:us-east-1:802587217904:destination:LGGXBmZw2Z47MmWq6b#VlGYyRJNfvVVgHf8y1#serverless-with-dashboard-playground-01#dev` is a [kinesis stream](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CreateDestination.html) within the serverless AWS account.

This is done via [Cross-Account Log Data Sharing with Subscriptions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CrossAccountSubscriptions.html)

---

**Serverless Dashboard | Views**

![](https://www.evernote.com/l/AAGYJOJTHApN_JpkCV9-zdcb715b6wEMRboB/image.png)

![](https://www.evernote.com/l/AAHPvh8KYGtDzp-FJpeJJkD9YQMDlMoV_ZkB/image.png)

![](https://www.evernote.com/l/AAEQi7li2WpL-4bLb1_Ot_Gu582YPZrYh5oB/image.png)

![](https://www.evernote.com/l/AAFGw-o8o3VJP5LHwE3xALCQtHh-0MvopoYB/image.png)

![](https://www.evernote.com/l/AAHb32bKNhNB47q-tjd9FcBhpH4x0q0TXwMB/image.png)

![](https://www.evernote.com/l/AAHv-DCGUmxCG6nBhHqcJWf7WY15kUwoX8cB/image.png)

![](https://www.evernote.com/l/AAFSDTovX61GYqcknC4dRdTFafjVuAvuWFIB/image.png)

![](https://www.evernote.com/l/AAGF3ML-wkFM1bPRMMGiUtvsKzXxGmI1E3gB/image.png)

![](https://www.evernote.com/l/AAHY5jTZcXpLPJJwKhIsKWgRblpvrmHgXlsB/image.png)

---

## Resources

* [reddit | Serverless Framework now supports full lifecycle on AWS](https://www.reddit.com/r/aws/comments/cghxro/serverless_framework_now_supports_full_lifecycle/)