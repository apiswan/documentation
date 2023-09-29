# Setting Up GoReplay as a Sidecar on AWS ECS

**Version:** 1.0  
**Last Updated:** 29 Sep 2023  
**Author:** ApiSwan

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Step 1: Set Up AWS Environment](#step-1-set-up-aws-environment)
4. [Step 2: Create an Amazon ECS Cluster](#step-2-create-an-amazon-ecs-cluster)
5. [Step 3: Containerize Your Main Application](#step-3-containerize-your-main-application)
6. [Step 4: Define ECS Task Definition](#step-4-define-ecs-task-definition)
7. [Step 5: Configure GoReplay](#step-5-configure-goreplay)
8. [Step 6: Deploy the Application with GoReplay](#step-6-deploy-the-application-with-goreplay)
9. [Step 7: Testing](#step-7-testing)
10. [Step 8: Monitoring and Logging](#step-8-monitoring-and-logging)
11. [Conclusion](#conclusion)

## Introduction

This document provides a step-by-step guide to setting up GoReplay as a sidecar alongside your main application in an Amazon Elastic Container Service (ECS) cluster on Amazon Web Services (AWS). GoReplay is a versatile tool for capturing and replaying HTTP traffic, useful for various debugging and testing scenarios.

## Prerequisites

Before you begin, ensure you have the following prerequisites in place:

- [ ] An AWS account with appropriate permissions.
- [ ] AWS Command Line Interface (CLI) installed and configured.
- [ ] A Dockerized version of your main application.
- [ ] Basic knowledge of AWS ECS and task definitions.

## Step 1: Set Up AWS Environment

Set up your AWS environment, including Virtual Private Cloud (VPC), subnets, security groups, and any necessary IAM roles. Ensure that you have the necessary permissions to create and manage resources in AWS.

## Step 2: Create an Amazon ECS Cluster

Create an Amazon ECS cluster where you will deploy your containers. Use the AWS CLI or the AWS Management Console to create the cluster. Make sure to configure EC2 instances or Fargate profiles for your cluster to run your containers.

## Step 3: Containerize Your Main Application

Dockerize your main application if it's not already containerized. Build the Docker image and push it to a container registry, such as Amazon Elastic Container Registry (ECR), or a similar registry.

## Step 4: Define ECS Task Definition

Create an ECS task definition that includes both your main application container and the GoReplay container as sidecars. Refer to the provided example JSON task definition for guidance.

**Task Definition JSON Example:**

```json
{
  "family": "my-app",
  "containerDefinitions": [
    {
      "name": "main-app",
      "image": "<your-ecr-repo>/main-app:latest",
      "memory": 512,
      "cpu": 256,
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80
        }
      ]
    },
    {
      "name": "goreplay-sidecar",
      "image": "goreplay/goreplay",
      "memory": 256,
      "cpu": 128,
      "portMappings": [
        {
          "containerPort": 80
        }
      ],
      "command": [
        "--input-raw", ":8080",
        "--input-raw-track-response",
        "--output-file", "swan-%d-%H-%M-%S.log"
      ]
    }
  ]
}
```

Adjust the task definition details, container names, images, memory, CPU, and port mappings as needed. In this example, GoReplay captures traffic on port 8080 and logs it as .log files.
## Step 5: Configure GoReplay

Configure GoReplay by specifying its command arguments within the ECS task definition. Customize GoReplay's options according to your specific use case, such as specifying capture and replay settings.

## Step 6: Deploy the Application with GoReplay

Create an ECS service and task definition using the provided ECS task definition. Deploy the application with GoReplay by running the following AWS CLI command:

```bash
aws ecs create-service --cluster my-cluster --service-name my-app-service --task-definition my-app --desired-count 2
```

Adjust cluster name, service name, task definition, and desired count according to your requirements.

## Step 7: Testing

Thoroughly test the setup to ensure that GoReplay is capturing and replaying traffic as expected. Use tools like curl or automated test suites to validate the behavior.

## Step 8: Monitoring and Logging

Set up monitoring and logging for your application and GoReplay to track their performance and behavior. You can use Amazon CloudWatch and other AWS services for this purpose.

## Conclusion

Congratulations! You have successfully set up GoReplay as a sidecar alongside your main application in an Amazon ECS cluster on AWS. Ensure that you monitor and maintain your deployment for ongoing performance and reliability.

For additional information and advanced configuration options, refer to the official GoReplay documentation and AWS ECS documentation.
