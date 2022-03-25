# Terraform

Terraform is a tool that allows you to define project infrastructure in code. This makes it possible to version-control configurations without simple snapshotting and distributing the entire environment.

## Infrastructure As Code (IAC)
"Infrastructure as code" represents the idea that everything needed to define, deploy, update, or destroy infrastructure should be capturable in code. There are several categories of IAC tools, which we'll explore.

### Ad Hoc Scripts

For example, a simple shell script can contain a number of commands to be run sequentially. This offers tremendous freedom, but it can quickly become difficult to maintain these scripts as application size grows.

### Configuration Management Tools
Examples: Chef, Puppet, Ansible, SaltStack.

These tools are designed to install and manage software on preexisting servers. They offer more consistent structure, and include functionality to ensure that repeated execution produces a consistent result (ie idempotency), as well as utilities to easily scale to distributed configurations.

### Server Templating Tools
Examples: Docker, Packer, Vagrant.

This class of tool defines an image of a server that includes everything needed to run the code. The idea behind server templating tools is to create an image of a server that captures a fully self-contained snapshot of the OS, the software, the files, and all other relevant details. Virtual Machines and Containers are two paradigms for working with these images.

### Orchestration Tools
Examples: Kubernetes, Marathon/Mesos, Amazon ECS, Docker Swarm, Nomad.

These tools allow you to deploy containers and VMS, strategically push out updates, scale up with load and balance that load, monitor the health of machines, and communicate with other containers.

### Provisioning Tools
Examples: Terraform, CloudFormation, OpenStack Heat.

Beyond just defining the code that runs on servers, provisioning tools handle creation of servers, as well as other resources (eg databases, caches, SSL certs, queues, etc), configurations (eg firewall settings, routing rules, subnet config, etc), and services (eg monitoring, load balancers, etc).


## Terraform

From your local/build machine, you can define infrastructure, compile that to a binary, then push that onto the platform of interest (eg AWS, GCP).

Terraform produces essentially immutable infrastructure using a declarative style and a masterless architecture (ie you don't need to maintain a separate machine to push commands to your servers).

# Using Terraform

[Tutorial using docker](https://learn.hashicorp.com/tutorials/terraform/docker-build?in=terraform/docker-get-started)

Terraform configurations will typically consist of at least a `main.tf` file.

### `main.tf`

This file defines the resources and "providers" for an infrastructure-setup.

* The terraform {} block contains Terraform settings, including the required providers Terraform will use to provision your infrastructure. For each provider, the source attribute defines an optional hostname, a namespace, and the provider type. 
* The provider block configures the specified provider, in this case docker. A provider is a plugin that Terraform uses to create and manage your resources.
    * [Providers registry](https://registry.terraform.io)

* Use resource blocks to define components of your infrastructure. A resource might be a physical or virtual component such as a Docker container, or it can be a logical resource such as a Heroku application.

Files in a directory can be formatted via the command `terraform fmt`.
A terraform configuration can be validated via the command `terraform validate`.

You can compile a terraform configuration via `terraform apply` and then by entering "yes" after reviewing the plan.

You can inspect a build via `terraform show`

















End