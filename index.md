# About Project-Env

## TL;TR
Project-Env automatically maintains the setup of project specific tools like a JDK or NodeJS.

## Description

### Problem
Before being able to start contributing to a project, I often have to follow project setup instructions. One thing, which is needed for almost every project, is installing required tools like a JDK or NodeJS. As I am working on multiple projects at the same time, the management of multiple versions of the same tool type gets complicated. If you for example have to install GraalVM for one project and AdoptOpenJDK for another, you have to ensure, that your IDE/Shell is always using the correct JDK in each project. It even gets more tedious if you have to maintain the tool versions in a CI/CD environment (e.g. in Jenkins through global tools).

Until today, I couldn't find any good solution which completely handles that part of a project and that's the reason why I created Project-Env.

### Solution
The idea is to have a configuration file in each project which specifies which tools are needed to work with the project. This file can then be processed by any tool like the IDE to set itself up with the required tools.

The goal of the Project-Env project is to define the format of the configuration file and to provide tools/plugins which allow the usage of Project-Env in a project (e.g. IntelliJ plugin).

### Current State
At the moment the Project-Env project is more a draft than a production ready toolset.

* [project-env-core](https://github.com/Project-Env/project-env-core): Java library which contains
    * an initial draft of the configuration file format in form of an YAML-Schema
    * Jackson bindings for the YAML-Schema
    * a utility to read a configuration file with Jackson
    * a utility to install the tools into a specific directory

* [project-env-shell](https://github.com/Project-Env/project-env-shell): Java CLI which allows to install the tools from a specified configuration file into a specific directory and to generate a Shell script to set up the tools in a shell environment (e.g. ZSH, Github Actions). The CLI is compiled into a standalone executable with the help of the GraalVM native image.

* [project-env-intellij](https://github.com/Project-Env/project-env-intellij-plugin): IntelliJ plugin which reads the configuration file, installs the tools and configures itself to use the installed tools (e.g. adds a new JDK and sets it as project JDK).
