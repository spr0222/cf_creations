# cf_creations

A hands-on practice repo for learning AWS infrastructure-as-code using CloudFormation, with GitHub Actions for automated deployments.

## What's inside

| File | What it creates |
|---|---|
| `infra/s3-template.yaml` | A simple S3 bucket |
| `infra/vpc-stack.yaml` | A VPC with public/private subnets, Internet Gateway, and NAT Gateway |
| `infra/vpc-sns_sqs_lambda-stack.yaml` | The VPC stack extended with an SNS → SQS → Lambda event pipeline |
| `infra/beanstalk-nodejs-stack.yaml` | A 2-AZ VPC, an Elastic Beanstalk Node.js environment (ALB + Auto Scaling group, Rolling deployments with a fixed batch size of 1), and an RDS database |

Each template in `.github/workflows/` deploys its corresponding stack to AWS (`us-east-1`) on push to `main`, using `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` repository secrets (IAM user access keys, configured manually — no OIDC federation).

The Elastic Beanstalk stack also requires a `DB_PASSWORD` repository secret, used as the RDS master password.

## Architecture progression

```
S3 bucket  →  VPC networking  →  Event-driven pipeline inside VPC   →  Elastic Beanstalk Node.js app
                                  (SNS → SQS → Lambda)                  (ALB + ASG + RDS)
```

The templates are intentionally ordered as a learning progression — each one builds on the previous.
