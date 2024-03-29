# <img src="https://github.com/apiswan/documentation/assets/138169260/aeb58b96-56d7-4c99-b0d2-46eb54bdfa49" width="24"> API Swan 


**Version:** 1.0  
**Last Updated:** 29 Sep 2023  
**Author:** ApiSwan

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Installation](#installation)
   - [Step 1: Setup Goreplay](#step-1-setup-goreplay)
   - [Step 2: APISwan Executables](#step-2-apiswan-executables)
   - [Step 3: APISwan - Client](#step-3-apiswan---client)
   - [Step 4: APISwan - Web App](#step-4-apiswan---app)
4. [Testing](#testing)

## Introduction

This document provides a step-by-step guide to setting up APISwan for automated regression testing alongside your main application.

## Prerequisites

Before you begin, ensure you have the following prerequisites in place:

- [ ] A Dockerized version of your main application.
- [ ] [Goreplay](https://github.com/buger/goreplay) running as a sidecar container along with the main application.
- [ ] APISwan client and UI binary executable files
- [ ] APISwan Client ID

## Installation
### Step 1: Setup Goreplay
Follow these guides to setup goreplay based on your existing tech stack:
1. [AWS - Using Elastic Container Service (ECS)](goreplay-ecs.md)
2. [AWS - Using Elastic Kubernetes Service (EKS)](goreplay-eks.md)
3. [GCP - Using Google Kubernetes Engine (GKE)](goreplay-gke.md)
4. [Docker](goreplay-docker.md)
5. [Kubernetes](goreplay-kubernetes.md)

### Step 2: APISwan Executables
For configuring APISwan, you will require the following executable files:
1. APISwan Client
2. APISwan App

### Step 3: APISwan - Client
The client's responsibilities include the following:
1. Monitoring the log file generated by GoReplay.
2. Creating and executing the regression test suite.

In addition to the client executable, you will need to create a `config.json` file to tailor the client's configuration to your specific needs. Here's an example configuration:

```json
{
    "encryption_key": "01234567890123456789012345678901",
    "replay_host_url": "http://localhost:8080",
    "client_id": "6513c8f4a5e88aec56f093dc",
    "client_port": "9080"
}
```

### Step 4: APISwan - Web App
Launch the app binary file to access the user interface.
```
serve -s -l 3000 apiswan-ui
```

## Testing

To simulate traffic, manually call various API endpoints of your application. Navigate to the app and trigger a "swan event" to observe the regression test in action.
