# Twingate + Coder
This repository provides an example of how to access Twingate protected resources in the Coder workspace.

## Introduction
[Coder](https://coder.com/) is an open-source platform for creating and managing developer workspaces on your preferred clouds and servers. 

This repository provides an example of how to configure a [Twingate](https://www.twingate.com/) client running with either a [service account](https://www.twingate.com/docs/services) or as a regular user in Coder. Users will then be able to access resources such as internal databases or APIs that are Twingate protected directly from their Coder workspace.
## Getting started
In this example, we will demonstrate how to set up Twingate client in Coder by modifying the Coder Docker default templates.

### Prerequisites
1. [Twingate](https://www.twingate.com/) account
2. [Coder server](https://coder.com/docs/v2/latest/install) installed

#### Connect using a regular Twingate account
1. Create a new Coder Docker starter template, for more details see [here](https://coder.com/docs/v2/latest/templates/tutorial#2-choose-a-starter-template) 
2. Replace the content of the default `main.tf` with [docker_interactive.tf](./templates/docker_interactive.tf), for more details see [here](https://coder.com/docs/v2/latest/templates/tutorial#6-modify-your-template)
3. Go to the Setting page of the template and set your Twingate Tenant Name in Variable, e.g. for `acme.twingate.com`, insert `acme`
4. Create workspace and open the workspace terminal. run `twingate status`
   1. If status is `authenticating` you should follow the URL displayed to authenticate
   2. If status is `not running`, execute `twingate start` followed by `/usr/bin/twingate-notifier console` and follow the URL displayed to authenticate

#### Connect using a Twingate service account
1. Generate a Twingate Service Account Key, for more details see [here](https://www.twingate.com/docs/services)
2. Create a new Docker starter template, for more details see [here](https://coder.com/docs/v2/latest/templates/tutorial#2-choose-a-starter-template)
3. Replace the content of the default `main.tf` with [docker_serviceaccount.tf](./templates/docker_serviceaccount.tf), for more details see [here](https://coder.com/docs/v2/latest/templates/tutorial#6-modify-your-template)
4. Go to the Setting page of the template and set your Twingate Service Key Variable
5. Create workspace and open the workspace terminal. run `twingate status` - it should return `online`

### How it works and other templates
To see how we modified the default Docker template and how this would work for other types of templates, e.g. Kubernetes, see [Other Template](docs/OTHER_TEMPLATE.md)

### Known Limitations
1. Twingate client requires `privileged` docker container