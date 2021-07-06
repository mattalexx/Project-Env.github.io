# About Project-Env

## TL;DR
Project-Env automatically maintains the project-local setup of project specific tools like a JDK or NodeJS.

## Problem

Before being able to start contributing to a project, I often have to follow project setup instructions. One thing, which is needed for almost every project, is installing and configuring required tools like a JDK or NodeJS. As I am working on multiple projects at the same time, the management of multiple versions of the same tool type gets complicated. If you for example have to install GraalVM for one project and AdoptOpenJDK for another, you have to ensure, that your IDE/Shell is always using the correct JDK in each project. It even gets more tedious if you have to maintain the tool versions in a CI/CD environment (e.g. in Jenkins through global tools).

Until today, I couldn't find any good solution which completely handles that part of a project (and still allows me to use my favourite IDE - IntelliJ). That's why I created Project-Env.

### My requirements
* Support for both Windows and macOS
* Support for project specific tool configuration &#8594; no side-effects in other projects
* Support for automatic configuration of installed tools in development tools/environments

## Idea

The idea is to have a simple configuration file in each project which specifies which tools are needed to work with the project. This file can then used by any tool to set itself up with the required third-party tools.

### Solution

Project-Env currently consists of the following repositories:

* [project-env-cli](https://github.com/Project-Env/project-env-cli): The raw CLI application which can be used by a Project-Env integration to setup all tools defined in the configuration file. As a result, the CLI will return a JSON describing the tools which were setup.
  
* [project-env-shell](https://github.com/Project-Env/project-env-shell): The shell integration application, which call the CLI to setup all tools and generates a Shell script to set up the tools in a shell environment (e.g. ZSH, Github Actions, Cygwin, ...).

* [project-env-intellij](https://github.com/Project-Env/project-env-intellij-plugin): The IntelliJ plugin, which call the CLI to setup all tools and configures IntelliJ to use the installed tools (e.g. adds a new JDK and sets it as project JDK. See  [README](https://github.com/Project-Env/project-env-intellij-plugin/blob/master/README.md) for more details).
  
#### Supported Tools
* JDK
* NodeJS
* Gradle
* Maven
* Generic tools which are portable and can be downloaded as a single archive (e.g. JAXB-RI)
* Git Hooks

### Similar projects/tools
* https://sdkman.io
* https://asdf-vm.com/#/core-manage-asdf
* https://github.com/volta-cli/volta
