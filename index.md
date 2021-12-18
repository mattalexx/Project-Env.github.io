---
layout: default
title: Introduction
description: Yet another project tools manager
nav_order: 1
---

# Introduction

## TL;DR
Project-Env automatically maintains the project-local setup of project specific tools like a JDK or NodeJS in a shell/IDE/CI environment.

## Problem

Before being able to start contributing to a project, I often have to follow project setup instructions. One thing, which is needed for almost every project, is installing and configuring required tools like a JDK or NodeJS. As I am working on multiple projects at the same time, the management of multiple versions of the same tool type gets complicated. If you for example have to install GraalVM for one project and AdoptOpenJDK for another, you have to ensure, that your IDE/Shell is always using the correct JDK in each project. It even gets more tedious if you have to maintain the tool versions in a CI environment (e.g. in Jenkins through global tools).

Until today, I couldn't find any solution which completely handles that part of a project. That's why I created Project-Env.

### Requirements for the solution
* Support for Windows, Linux and macOS
* Support for project specific tool configuration &#8594; no side effects in other projects
* Support for automatic configuration of installed tools in development tools/environments

## Idea

The idea is to have a simple configuration file in each project which specifies which tools are needed to work with the project. This file can then be used by any tool to set itself up with the required third-party tools.

## Similar projects/tools
* <https://sdkman.io>{:target="_blank"}
* <https://asdf-vm.com>{:target="_blank"}
* <https://volta.sh>{:target="_blank"}

--- 

[Getting started](./docs/getting-started.md){: .btn }