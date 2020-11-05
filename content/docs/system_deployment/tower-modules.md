---
title: Tower Modules
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
    weight: 2
---

The application is composed of different modules that can be configured and deployed depending on user requirements.

All components are packaged as Docker container images which are hosted and security validated by the Amazon ECR service.

### Backend module

The backend is implemented as a JVM-based application server based on the Micronaut framework which provides a modern and secure backbone for the application server.

It requires OpenJDK 8 or later.

The backend layer implements the main application logic organised in a *service* layer, which is then exposed via a REST API and defined via an OpenAPI schema.

The backend module uses JPA/Hibernate/JDBC API industry standards to connect the underlying relational database.

The backend is designed to run standalone or multiple replicas for scalability when deployed in high-availability mode.  

### Frontend module

The frontend module is composed by Angular 8 application which is served by an Nginx web server.

The frontend can be configured to expose the application directly to the user/DMZ via an HTTPS connection or through a load balancer.

### Storage

Nextflow Tower requires a relational database as its primary storage.

It is suggested to use MySQL 5.6, however, any SQL database compatible with JPA/JDBC industry-standards are supported.

### Caching

Tower provides an optional caching module for configurations requiring high availability.

This module requires a Redis 5.0 in-memory database.

### Authentication module

Nextflow Tower supports enterprise authentication mechanisms such as OAuth and LDAP.

Third-party authority providers and custom single-sign-on flow can be developed depending on exact customer requirements.

### Cron scheduler

Tower implements a cron service which takes care of executing periodical activities, such as sending e-mail notifications and cleaning up.

The cron service can be configured to run as an embedded backend service or an independent service.
