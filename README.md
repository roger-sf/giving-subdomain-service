# WIP

## Setup

```bash
npm install
```

## Deploy

In order to deploy the endpoint run

```bash
serverless deploy
```

The expected result should be similar to:

```bash
Serverless: Packaging service…
Serverless: Uploading CloudFormation file to S3…
Serverless: Uploading service .zip file to S3…
Serverless: Updating Stack…
Serverless: Checking Stack update progress…
Serverless: Stack update finished…

Service Information
service: subdomain-service
stage: dev
region: us-east-1
api keys:
  None
endpoints:
  POST - https://45wf34z5yf.execute-api.us-east-1.amazonaws.com/dev/subdomainMapping
  GET - https://45wf34z5yf.execute-api.us-east-1.amazonaws.com/dev/subdomainMapping
  GET - https://45wf34z5yf.execute-api.us-east-1.amazonaws.com/dev/subdomainMapping/{id}
  PUT - https://45wf34z5yf.execute-api.us-east-1.amazonaws.com/dev/subdomainMapping/{id}
  DELETE - https://45wf34z5yf.execute-api.us-east-1.amazonaws.com/dev/subdomainMapping/{id}
functions:
  subdomain-service-update: arn:aws:lambda:us-east-1:488110005556:function:subdomain-service-update
  subdomain-service-get: arn:aws:lambda:us-east-1:488110005556:function:subdomain-service-get
  subdomain-service-list: arn:aws:lambda:us-east-1:488110005556:function:subdomain-service-list
  subdomain-service-register: arn:aws:lambda:us-east-1:488110005556:function:subdomain-service-register
  subdomain-service-delete: arn:aws:lambda:us-east-1:488110005556:function:subdomain-service-delete
```

## Usage

### Validate a subdomain

```bash
curl https://XXXXXXX.execute-api.us-east-1.amazonaws.com/dev/subdomainMapping/isAvailable/redcross
```

Example Result:
```bash
{"subdomain":"redcross","isValid":true,"isBlacklisted":false,"isTaken":false}%
```


### register a subdomainMapping

```bash
curl -X POST https://XXXXXXX.execute-api.us-east-1.amazonaws.com/dev/subdomainMapping --data '{ "subdomain": "redcross" }'
```

Example Result:
```bash
{"subdomain":"redcross","id":"ee6490d0-aa11e6-9ede-afdfa051af86","registerdAt":1479138570824,"updatedAt":1479138570824}%
```

### List all subdomainMapping

```bash
curl https://XXXXXXX.execute-api.us-east-1.amazonaws.com/dev/subdomainMapping
```

Example output:
```bash
[{"subdomain":"redcross","id":"ac90feaa11e6-9ede-afdfa051af86","checked":true,"updatedAt":1479139961304},{"subdomain":"redcross","id":"206793aa11e6-9ede-afdfa051af86","registerdAt":1479139943241,"checked":false,"updatedAt":1479139943241}]%
```

### Get one subdomainMapping

```bash
# Replace the <id> part with a real id from your subdomainMapping table
curl https://XXXXXXX.execute-api.us-east-1.amazonaws.com/dev/subdomainMapping/<id>
```

Example Result:
```bash
{"subdomain":"redcross","id":"ee6490d0-aa11e6-9ede-afdfa051af86","registerdAt":1479138570824,"checked":false,"updatedAt":1479138570824}%
```

### Update a subdomainMapping

```bash
# Replace the <id> part with a real id from your subdomainMapping table
curl -X PUT https://XXXXXXX.execute-api.us-east-1.amazonaws.com/dev/subdomainMapping/<id> --data '{ "subdomain": "redcross", "checked": true }'
```

Example Result:
```bash
{"subdomain":"redcross","id":"ee6490d0-aa11e6-9ede-afdfa051af86","registerdAt":1479138570824,"checked":true,"updatedAt":1479138570824}%
```

### Delete a subdomainMapping

```bash
# Replace the <id> part with a real id from your subdomainMapping table
curl -X DELETE https://XXXXXXX.execute-api.us-east-1.amazonaws.com/dev/subdomainMapping/<id>
```

No output

## Scaling

### AWS Lambda

By default, AWS Lambda limits the total concurrent executions across all functions within a given region to 100. The default limit is a safety limit that protects you from costs due to potential runaway or recursive functions during initial development and testing. To increase this limit above the default, follow the steps in [To request a limit increase for concurrent executions](http://docs.aws.amazon.com/lambda/latest/dg/concurrent-executions.html#increase-concurrent-executions-limit).

### DynamoDB

When you register a table, you specify how much provisioned throughput capacity you want to reserve for reads and writes. DynamoDB will reserve the necessary resources to meet your throughput needs while ensuring consistent, low-latency performance. You can change the provisioned throughput and increasing or decreasing capacity as needed.

This is can be done via settings in the `serverless.yml`.

```yaml
  ProvisionedThroughput:
    ReadCapacityUnits: 1
    WriteCapacityUnits: 1
```


