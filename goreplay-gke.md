---
title: Goreplay GKE
layout: template
filename: goreplay-gke.md
--- 
# Setting Up GoReplay as a Sidecar on Google Cloud Platform

**Version:** 1.0  
**Last Updated:** 29 Sep 2023  
**Author:** ApiSwan

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Step 1: Enable Google Kubernetes Engine (GKE)](#step-1-enable-google-kubernetes-engine-gke)
4. [Step 2: Create a GKE Cluster](#step-2-create-a-gke-cluster)
5. [Step 3: Containerize Your Main Application](#step-3-containerize-your-main-application)
6. [Step 4: Create Kubernetes Deployment](#step-4-create-kubernetes-deployment)
7. [Step 5: Configure GoReplay](#step-5-configure-goreplay)
8. [Step 6: Deploy the Application with GoReplay](#step-6-deploy-the-application-with-goreplay)
9. [Step 7: Testing](#step-7-testing)
10. [Step 8: Monitoring and Logging](#step-8-monitoring-and-logging)
11. [Conclusion](#conclusion)

## Introduction

This document provides a step-by-step guide to setting up GoReplay as a sidecar alongside your main application in a Google Kubernetes Engine (GKE) cluster on Google Cloud Platform (GCP). GoReplay is a versatile tool for capturing and replaying HTTP traffic, useful for various debugging and testing scenarios.

## Prerequisites

Before you begin, ensure you have the following prerequisites in place:

- [ ] A Google Cloud Platform (GCP) account.
- [ ] The Google Cloud SDK (gcloud) installed and configured.
- [ ] A Dockerized version of your main application.
- [ ] Basic knowledge of Kubernetes and GKE.

## Step 1: Enable Google Kubernetes Engine (GKE)

Enable the Google Kubernetes Engine API in your GCP project if it's not already enabled using the following command:

```shell
gcloud services enable container.googleapis.com
```

## Step 2: Create a GKE Cluster

Create a GKE cluster where you will deploy your containers:

```shell
gcloud container clusters create my-cluster \
  --num-nodes=2 \
  --machine-type=n1-standard-2 \
  --zone=us-central1-a
```

Adjust the cluster name, number of nodes, machine type, and zone according to your requirements.

## Step 3: Containerize Your Main Application

Dockerize your main application if it's not already containerized. Build the Docker image and push it to Google Container Registry (GCR) or a similar container registry.

## Step 4: Create Kubernetes Deployment

Create a Kubernetes deployment configuration that includes both your main application container and the GoReplay container as sidecars. Refer to the provided example YAML configuration for guidance.

**Deployment YAML Example:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: main-app
          image: gcr.io/your-project/main-app:latest
        - name: goreplay-sidecar
          image: goreplay/goreplay
          args:
            - "--input-raw :8080"
            - "--input-raw-track-response"
            - "--output-file swan-%d-%H-%M-%S.log"
```

Adjust the container names, images, and arguments as needed. In this example, GoReplay captures traffic on port 8080 and logs it as .log files.

## Step 5: Configure GoReplay

Configure GoReplay by specifying its arguments within the Kubernetes deployment configuration. Customize GoReplay's options according to your specific use case, such as specifying capture and replay settings.

## Step 6: Deploy the Application with GoReplay

Apply the deployment configuration to create the pods with your main application and GoReplay sidecar:

```shell
kubectl apply -f deployment.yaml
```

## Step 7: Testing

Thoroughly test the setup to ensure that GoReplay is capturing and creating the log files. Use tools like curl or automated test suites to validate the behavior.

## Step 8: Monitoring and Logging

Set up monitoring and logging for your application and GoReplay to track their performance and behavior. You can use Google Cloud Monitoring and Logging for this purpose.

## Conclusion

Congratulations! You have successfully set up GoReplay as a sidecar alongside your main application in a GKE cluster on GCP. Ensure that you monitor and maintain your deployment for ongoing performance and reliability.

For additional information and advanced configuration options, refer to the official GoReplay documentation and Google Kubernetes Engine documentation.
