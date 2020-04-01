# S3BasicAuthDemo

## Overview

- Amazon S3 での静的ウェブサイトのホスティングするデモ
- CloudFront を使用してAmazon S3 での静的ウェブサイトを提供するデモ

## Amazon S3 での静的ウェブサイトのホスティングする

### Deploy

```sh
STACK_NAME=s3-web-hosting-demo

aws cloudformation create-stack \
    --stack-name ${STACK_NAME} \
    --capabilities CAPABILITY_IAM \
    --template-body file://template-${STACK_NAME}.yaml
```

```sh
S3_BUCKET=$(aws cloudformation describe-stacks \
    --stack-name ${STACK_NAME} \
    --query 'Stacks[].Outputs[?OutputKey==`S3Bucket`].OutputValue' \
    --output text)
echo ${S3_BUCKET}

cat << EOT | tee index.html
${STACK_NAME}
EOT

aws s3 cp index.html s3://${S3_BUCKET}
```

### Website URL

```sh
aws cloudformation describe-stacks \
    --stack-name ${STACK_NAME} \
    --query 'Stacks[].Outputs[?OutputKey==`WebsiteURL`].OutputValue' \
    --output text
```

### Undeploy

```sh
aws s3 rm s3://${S3_BUCKET} --recursive
aws cloudformation delete-stack --stack-name ${STACK_NAME}
```

## CloudFront を使用してAmazon S3 での静的ウェブサイトを提供する

### Deploy

```sh
STACK_NAME=s3-cloudfront-demo

aws cloudformation create-stack \
    --stack-name ${STACK_NAME} \
    --capabilities CAPABILITY_IAM \
    --template-body file://template-${STACK_NAME}.yaml
```

```sh
S3_BUCKET=$(aws cloudformation describe-stacks \
    --stack-name ${STACK_NAME} \
    --query 'Stacks[].Outputs[?OutputKey==`S3Bucket`].OutputValue' \
    --output text)
echo ${S3_BUCKET}

cat << EOT | tee index.html
${STACK_NAME}
EOT

aws s3 cp index.html s3://${S3_BUCKET}
```

### Website URL

```sh
aws cloudformation describe-stacks \
    --stack-name ${STACK_NAME} \
    --query 'Stacks[].Outputs[?OutputKey==`WebsiteURL`].OutputValue' \
    --output text
```

### Undeploy

```sh
aws s3 rm s3://${S3_BUCKET} --recursive
aws cloudformation delete-stack --stack-name ${STACK_NAME}
```
