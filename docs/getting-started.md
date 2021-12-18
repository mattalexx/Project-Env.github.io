---
layout: default
title: Getting started
description: Yet another project tools manager
nav_order: 2
---

# Getting started
To simplify integrations, I created a [CLI application](./docs/cli.md) which contains the heavy part of Project-Env, which is the setup and configuration of tools defined in a Project-Env configuration file. The returned tools info can then be used by an integration to configure them in the corresponding environment.

## Set up config file
To get started with Project-Env in a project, you first need to set up the Project-Env config file. Since the configuration options are strongly linked to the used [CLI application](./cli.md) version, the options are documented at the same location as the one for the CLI.

## Set up CLI application
If you want to use Project-Env on your local development machine, you need to download the [CLI application](./cli.md) and make it available through the `PATH` variable (there exist some automatic installation options too). Integrations like the one for Github Actions or the Jenkins Pipeline plugin will set up the CLI automatically.

## Set up integrations
Choose one or more [integrations](./integrations/index.md) and set them up according to the corresponding documentation.