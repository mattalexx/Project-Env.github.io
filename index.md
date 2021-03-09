# About Project-Env

## TL;DR
Project-Env automatically maintains the local setup of project specific tools like a JDK or NodeJS.

## Problem

Before being able to start contributing to a project, I often have to follow project setup instructions. One thing, which is needed for almost every project, is installing required tools like a JDK or NodeJS. As I am working on multiple projects at the same time, the management of multiple versions of the same tool type gets complicated. If you for example have to install GraalVM for one project and AdoptOpenJDK for another, you have to ensure, that your IDE/Shell is always using the correct JDK in each project. It even gets more tedious if you have to maintain the tool versions in a CI/CD environment (e.g. in Jenkins through global tools).

Until today, I couldn't find any good solution which completely handles that part of a project (and still allows me to use my favourite IDE - IntelliJ). That's the reason why I created Project-Env.

## Idea

The idea is to have a simple configuration file in each project which specifies which tools are needed to work with the project. This file can then be processed by any tool like the IDE or Shell to set itself up with the required tools.

The goal of the Project-Env project is to define the format of the configuration file and to provide tools/plugins which allow the usage of Project-Env in a project (e.g. through a IntelliJ plugin).

### Solution

Project-Env consists of the following repositories:

* [project-env-shell](https://github.com/Project-Env/project-env-shell): Java CLI which reads the Project-Env configuration file, installs the tools and generates a Shell script to set up the tools in a shell environment (e.g. ZSH or Github Actions). The CLI is compiled into a standalone executable with the help of the GraalVM native image.

* [project-env-intellij](https://github.com/Project-Env/project-env-intellij-plugin): IntelliJ plugin which reads the Project-Env configuration file, installs the tools and configures itself to use the installed tools (e.g. adds a new JDK and sets it as project JDK - see  [README](https://github.com/Project-Env/project-env-intellij-plugin/blob/master/README.md) for more details).

* [project-env-core](https://github.com/Project-Env/project-env-core): Represents the common functionality of the prior repositories:
  * Jackson bindings for the Project-Env configuration YAML-Schema
  * a utility to read a configuration file with the Jackson bindings
  * a utility to install the tools into a specific directory
  
#### Supported Tools
* JDK
* NodeJS
* Gradle
* Maven
* Generic tools which are portable and can be downloaded as a single archive (e.g. JAXB-RI)
* Git Hooks

#### YAML-Schema

```
{
    "$schema": "http://json-schema.org/draft/2019-09/schema#",
    "title": "Project Env Configuration",
    "type": "object",
    "additionalProperties": false,
    "properties": {
        "tools": {
            "$ref": "#/definitions/ToolsConfiguration"
        }
    },
    "required": [
        "tools"
    ],
    "definitions": {
        "ToolsConfiguration": {
            "type": "object",
            "additionalProperties": false,
            "properties": {
                "toolsDirectory": {
                    "type": "string"
                },
                "jdk": {
                    "$ref": "#/definitions/JdkConfiguration"
                },
                "maven": {
                    "$ref": "#/definitions/MavenConfiguration"
                },
                "gradle": {
                    "$ref": "#/definitions/GradleConfiguration"
                },
                "node": {
                    "$ref": "#/definitions/NodeConfiguration"
                },
                "gitHooks": {
                    "$ref": "#/definitions/GitHooksConfiguration"
                },
                "genericTools": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/GenericToolConfiguration"
                    }
                }
            },
            "required": [
                "toolsDirectory"
            ]
        },
        "JdkConfiguration": {
            "type": "object",
            "additionalProperties": false,
            "properties": {
                "downloadUris": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/DownloadUri"
                    }
                },
                "environmentVariables": {
                    "type": "object",
                    "additionalProperties": {
                        "type": "string"
                    }
                },
                "pathElements": {
                    "type": "array",
                    "items": {
                        "type": "string"
                    }
                },
                "postExtractionCommands": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/PostExtractionCommand"
                    }
                }
            },
            "required": [
                "downloadUris"
            ]
        },
        "DownloadUri": {
            "type": "object",
            "additionalProperties": false,
            "properties": {
                "downloadUri": {
                    "type": "string"
                },
                "targetOs": {
                    "type": "string",
                    "enum": [
                        "ALL",
                        "MACOS",
                        "WINDOWS",
                        "LINUX"
                    ]
                }
            },
            "required": [
                "downloadUri"
            ]
        },
        "PostExtractionCommand": {
            "type": "object",
            "additionalProperties": false,
            "properties": {
                "executableName": {
                    "type": "string"
                },
                "arguments": {
                    "type": "array",
                    "items": {
                        "type": "string"
                    }
                }
            },
            "required": [
                "executableName"
            ]
        },
        "MavenConfiguration": {
            "type": "object",
            "additionalProperties": false,
            "properties": {
                "downloadUris": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/DownloadUri"
                    }
                },
                "environmentVariables": {
                    "type": "object",
                    "additionalProperties": {
                        "type": "string"
                    }
                },
                "pathElements": {
                    "type": "array",
                    "items": {
                        "type": "string"
                    }
                },
                "postExtractionCommands": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/PostExtractionCommand"
                    }
                },
                "globalSettingsFile": {
                    "type": "string"
                },
                "userSettingsFile": {
                    "type": "string"
                }
            },
            "required": [
                "downloadUris"
            ]
        },
        "GradleConfiguration": {
            "type": "object",
            "additionalProperties": false,
            "properties": {
                "downloadUris": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/DownloadUri"
                    }
                },
                "environmentVariables": {
                    "type": "object",
                    "additionalProperties": {
                        "type": "string"
                    }
                },
                "pathElements": {
                    "type": "array",
                    "items": {
                        "type": "string"
                    }
                },
                "postExtractionCommands": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/PostExtractionCommand"
                    }
                }
            },
            "required": [
                "downloadUris"
            ]
        },
        "NodeConfiguration": {
            "type": "object",
            "additionalProperties": false,
            "properties": {
                "downloadUris": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/DownloadUri"
                    }
                },
                "environmentVariables": {
                    "type": "object",
                    "additionalProperties": {
                        "type": "string"
                    }
                },
                "pathElements": {
                    "type": "array",
                    "items": {
                        "type": "string"
                    }
                },
                "postExtractionCommands": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/PostExtractionCommand"
                    }
                }
            },
            "required": [
                "downloadUris"
            ]
        },
        "GitHooksConfiguration": {
            "type": "object",
            "additionalProperties": false,
            "properties": {
                "directory": {
                    "type": "string"
                }
            }
        },
        "GenericToolConfiguration": {
            "type": "object",
            "additionalProperties": false,
            "properties": {
                "downloadUris": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/DownloadUri"
                    }
                },
                "environmentVariables": {
                    "type": "object",
                    "additionalProperties": {
                        "type": "string"
                    }
                },
                "pathElements": {
                    "type": "array",
                    "items": {
                        "type": "string"
                    }
                },
                "postExtractionCommands": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/PostExtractionCommand"
                    }
                }
            },
            "required": [
                "downloadUris"
            ]
        }
    }
}
```
