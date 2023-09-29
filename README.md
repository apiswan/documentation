# API Swan

**Version:** 1.0  
**Last Updated:** 29 Sep 2023  
**Author:** ApiSwan

## Table of Contents

1. [Introduction](#1-introduction)
2. [Prerequisites](#2-prerequisites)
3. [Step 1: Setup Goreplay](#3-step-1-setup-goreplay)
4. [Step 2: APISwan Executables](#4-step-2-apiswan-executables)
5. [Step 3: APISwan - Client](#5-step-3-apiswan---client)
6. [Step 4: APISwan - App](#6-step-4-apiswan---app)
7. [Step 5: Testing](#7-step-5-testing)

## 1. Introduction

This document provides a step-by-step guide to setting up APISwan for automated regression testing alongside your main application.

## 2. Prerequisites

Before you begin, ensure you have the following prerequisites in place:

- [ ] A Dockerized version of your main application.
- [ ] Goreplay running as a sidecar container along with the main application.
- [ ] APISwan client and UI binary executable files
- [ ] APISwan Client ID

## 3. Step 1: Setup Goreplay
Follow these guides to setup goreplay based on your existing tech stack:
1. [AWS - Using Elastic Container Service (ECS)](#introduction)
2. [AWS - Using Elastic Kubernetes Service (EKS)](#prerequisites)
3. [GCP - Using Google Kubernetes Engine (GKE)](#step-1-enable-google-kubernetes-engine-gke)

## 4. Step 2: APISwan Executables
To setup APISwan, you need the following binary files:
1. APISwan Client
2. APISwan App

## 5. Step 3: APISwan - Client
The client is responsible for the following:
1. Scanning the log file generated by goreplay
2. Creation and executiong of the regression test suite.

Along with the client, you need to create a config.json files to configure the client based on your requirements.
```json
{
    "encryption_key": "01234567890123456789012345678901",
    "replay_host_url": "http://localhost:8080",
    "client_id": "6513c8f4a5e88aec56f093dc",
    "client_port": "9080"
}
```

## 6. Step 4: APISwan - App
Run the app binary file to access the UI.


## 7. Step 5: Testing

Simulate traffic by manually calling different API endpoints of your application. Head over to the app and trigger a swan event to see the regression test in action.
