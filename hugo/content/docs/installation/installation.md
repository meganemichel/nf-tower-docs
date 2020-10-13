---
title: System deployment
weight: 1
layout: single
publishdate: 2020-06-11 04:00:00 +0000
expirydate: 2030-01-01 04:00:00 +0000
date: 2020-06-11 04:00:00 +0000
authors:
- Forestry Team
headline: ''
description: ''
textline: ''
categories: []
tags: []
cta:
  headline: ''
  textline: ''
  calls_to_action: []
private: false
images:
- "/uploads/2018/01/OGimage-01-docs-3x.jpg"
menu:
  docs:
    parent: Getting Started
    weight: 2
---

## Overview

Nextflow Tower is an open source monitoring and managing platform for Nextflow workflows. Source and full instructions are available [here](https://github.com/seqeralabs/nf-tower).


{{% tip %}}

[**Sign up**](https://tower.nf "Nextflow Tower") to Tower.nf for free today, or request a [**demo**](https://seqera.io/demo "Nextflow Tower Demo") for a deployment in your own on-premise or cloud environment.

{{% /tip %}}

{{% pretty_screenshot img="/uploads/2020/10/installation_reference_architecture.png" %}}

This diagram shows the Tower architecture deployed on AWS.


# Tower Modules

The application is composed of different modules that can be configured and deployed depending on user requirements.

All components are packaged as Docker container images which are hosted and security validated by the Amazon ECR service.  

## Backend module

The backend is implemented as a JVM-based application server based on the Micronaut framework which provides a modern and secure backbone for the application server.

It requires OpenJDK 8 or later.

The backend layer implements the main application logic organised in a *service* layer, which is then exposed via a REST API and defined via an OpenAPI schema.

The backend module uses JPA/Hibernate/JDBC API industry standards to connect the underlying relational database.

The backend is designed to run standalone or multiple replicas for scalability when deployed in high-availability mode.  

## Frontend module

The frontend module is composed by Angular 8 application which is served by an Nginx web server.

The frontend can be configured to expose the application directly to the user/DMZ via an HTTPS connection or through a load balancer.

## Storage

Nextflow Tower requires a relational database as its primary storage.

It is suggested to use MySQL 5.6, however, any SQL database compatible with JPA/JDBC industry-standards are supported.   

## Caching

Tower provides an optional caching module for configurations requiring high availability.

This module requires a Redis 5.0 in-memory database.

## Authentication module

Nextflow Tower supports enterprise authentication mechanisms such as OAuth and LDAP.

Third-party authority providers and custom single-sign-on flow can be developed depending on exact customer requirements.     

## Cron scheduler

Tower implements a cron service which takes care of executing periodical activities, such as sending mail notifications and cleaning up.

The cron service can be configured to run as an embedded backend service or an independent service.


# Deployment configurations

We created Nextflow in 2013 to deliver the most seamless experience for data pipelines at scale.

Nextflow Tower is the continuation of this mission encompassing the latest technologies.

## Basic deployment

In the minimal configuration only requires the front-end, backend and database module.

This can be executed as Docker containers or as native services running in the hosting environment.

This is generally a suggested configuration for evaluation purposes or for a small number of users.

### Kubernetes deployment

Kubernetes cluster management is emerging as the technology of choice for the deployment applications requiring high-availability, scalability and security standards.

Nextflow Tower Enterprise includes configuration manifests for the deployment in the Kubernetes environment.

## Nextflow client

The Nextflow workflow runtime includes a thin HTTP client to connect and submit execution metadata to the Tower service.

The service endpoint can be accessed at the backend layer or at the frontend layer depending on the chosen Tower configuration.

In both cases, the data transfer is encrypted using HTTPS protocol.  
