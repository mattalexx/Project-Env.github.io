---
layout: default
title: Project-Env
description: Yet another project tools manager
---

# TL;DR
Project-Env automatically maintains the project-local setup of project specific tools like a JDK or NodeJS in a shell/IDE/CI environment.

# Problem

Before being able to start contributing to a project, I often have to follow project setup instructions. One thing, which is needed for almost every project, is installing and configuring required tools like a JDK or NodeJS. As I am working on multiple projects at the same time, the management of multiple versions of the same tool type gets complicated. If you for example have to install GraalVM for one project and AdoptOpenJDK for another, you have to ensure, that your IDE/Shell is always using the correct JDK in each project. It even gets more tedious if you have to maintain the tool versions in a CI environment (e.g. in Jenkins through global tools).

Until today, I couldn't find any solution which completely handles that part of a project. That's why I created Project-Env.

## Requirements for the solution
* Support for Windows, Linux and macOS
* Support for project specific tool configuration &#8594; no side effects in other projects
* Support for automatic configuration of installed tools in development tools/environments

# Idea

The idea is to have a simple configuration file in each project which specifies which tools are needed to work with the project. This file can then be used by any tool to set itself up with the required third-party tools.

# Solution

To simplify the integration, I created the [project-env-cli](https://github.com/Project-Env/project-env-cli), a command line application which contains the heavy part of Project-Env, which is the set-up and configuration of tools defined in a Project-Env configuration file.

Currently, the following integrations exists for Project-Env: 
* [project-env-shell](https://github.com/Project-Env/project-env-shell): Shell integration application, which call the CLI to set up all tools and generates a Shell script to set up the tools in a shell environment (e.g. ZSH, Cygwin, ...).
* [project-env-intellij](https://github.com/Project-Env/project-env-intellij-plugin): IntelliJ plugin, which calls the CLI to set up all tools and configures IntelliJ to use the installed tools.
* [project-env-github-action](https://github.com/Project-Env/project-env-github-action): GitHub action, which calls the CLI to set up all tools and configures the runner environment to use the installed tools.

# Next steps
Project-Env is still under construction, but contains the most important features I need for my daily business. Probably, the next integration will be a Jenkins pipeline plugin, as this is still a black spot in a some projects I am working on.

# Similar projects/tools
* https://sdkman.io
* https://asdf-vm.com
* https://volta.sh