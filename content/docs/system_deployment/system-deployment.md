---
title: Deployment configurations
aliases:
- "/docs/system-deployment"
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
authors:
  - "Evan Floden"
  - "Alain Coletta"
  - "Seqera Labs"

headline: 'Deployment Guide'
description: 'System description and instructions for Nextflow Tower.'
menu:
  docs:
    parent: System deployment
    weight: 1
---

{{% tip %}}

It is highly recommended to first [**Sign up**](https://tower.nf "Nextflow Tower") and try the hosted version of Tower for free or request a [**demo**](https://seqera.io/demo "Nextflow Tower Demo") for a deployment in your own on-premise or cloud environment.

{{% /tip %}}

Nextflow Tower is a web application server based on a microservices oriented architecture and designed to maximize the portability, scalability and security of the application.

The application is composed of different modules that can be configured and deployed depending on user requirements.

All components for the Enterprise release are packaged as Docker container images which are hosted and security validated by the Amazon ECR service. The community version can be accessed via GitHub.

# Deployment configurations

### Basic deployment

The minimal Tower configuration only requires the front-end, backend and database modules.

These can be executed as Docker containers or as native services running in the hosting environment. Such minimal configuration is only suggested for evaluation purposes or for a small number of users.

### Kubernetes deployment

Kubernetes cluster management is emerging as the technology of choice for the deployment of applications requiring high-availability, scalability and security standards.

Nextflow Tower Enterprise includes configuration manifests for the deployment in the Kubernetes environment.

This diagram shows the system architecture for the reference deployment on AWS.

{{% pretty_screenshot img="/uploads/2020/10/installation_reference_architecture.png" %}}
