# DynamoDB with Next.js API Routes

![LocalStack Community](https://img.shields.io/badge/LocalStack-Community-green)
![Integration Pulumi](https://img.shields.io/badge/Integration-NextJS-black)

In this example, we will demonstrate how to use [DynamoDB](https://aws.amazon.com/dynamodb/) with [Next.js](https://nextjs.org/) API Routes. We will use LocalStack to emulate DynamoDB locally with AWS SDK for JavaScript.

## Prerequisites

- LocalStack
- Node.js & [`pnpm`](https://pnpm.io/installation)
- Docker
- `awslocal` CLI

## Instructions

You can build and deploy the sample application on LocalStack by running our Makefile commands. To deploy the infrastructure, you can run `make deploy` after installing the application dependencies. Here are instructions to deploy and test it manually step-by-step.

### Start LocalStack

Start your LocalStack container using the following command:

```bash
EXTRA_CORS_ALLOWED_ORIGINS=* localstack start
```

The `EXTRA_CORS_ALLOWED_ORIGINS` environment variable is used to allow CORS requests from any origin. This is required for the Next.js application to be able to upload images to the S3 bucket.

### Create a DynamoDB Table

Before we use DynamoDB for creating, updating, and deleting documents, we need to create a DynamoDB table. You can use the following command to create a DynamoDB table named `nextjs-api-routes`:

```bash
awslocal dynamodb create-table \
  --table-name nextjs-api-routes \
  --attribute-definitions AttributeName=id,AttributeType=S \
  --key-schema AttributeName=id,KeyType=HASH \
  --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5
```

You can edit the DynamoDB table name in the above command and the `TABLE_NAME` in the `.env` file. 

### Install the Dependencies

Install the dependencies using `pnpm`:

```bash
pnpm install
```

### Start the Application

Start the application using the following command:

```bash
pnpm run dev
```

It will start the Next.js application at [`http://localhost:3000`](http://localhost:3000).

### Test the Application

You can test the application using the following commands:

| HTTP Method | Command |
|-------------|---------|
| `PUT`         | `curl -X PUT http://localhost:3000/api/item -d '{"content": "test"}' -H "Content-type: application/json"` |
| `GET`         | `curl http://localhost:3000/api/item\?id\=bdc38386-2b35-47a3-bdfc-8ee29bd0686f` |
| `POST`        | `curl -X POST http://localhost:3000/api/item -d '{"content": "updated", "id": "bdc38386-2b35-47a3-bdfc-8ee29bd0686f"}' -H "Content-type: application/json"` |
| `DELETE`      | `curl -X DELETE http://localhost:3000/api/item\?id\=bdc38386-2b35-47a3-bdfc-8ee29bd0686f` |
