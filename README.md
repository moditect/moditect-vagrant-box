# ModiTect Vagrant Box

This is a Vagrant configuration for setting up a virtual machine based Fedora 25, containing
all tools required for experimenting with ModiTect, esp. running modular runtime images
created by ModiTect on Docker.

The following tools are installed:

* Java 8 and Java 9
* Maven
* git
* Docker

Ansible is used for provisioning the VM, see _playbook.yml_ for the complete set-up.

## Installation

Clone this repository:

    git clone https://github.com/moditect/moditect-vagrant-box.git

Change into the project directory, bootstrap the VM and SSH into it:

    cd moditect-vagrant-box
    vagrant up
    vagrant ssh

## Creating a modular runtime image based on Docker

Within the VM, clone the ModiTect repository and build ModiTect:

    git clone https://github.com/moditect/moditect.git
    cd moditect
    mvn clean install

Build the Docker image for the Undertow example:

    mvn clean install -pl :moditect-integrationtest-undertow -Pdocker

Run a Docker container based on that image:

    docker run --rm -t -i -p 8080:8080 moditect/undertow-helloworld

Access the Undertow service from your host machine at http://localhost:5678/.

## Using the Fedora 26 Minimal Base image

The Undertow example uses CentOS as a base for the Docker image. To use the
Fedora 26 Minimal Base image instead, fetch and load that image:

    wget -O - http://dl.fedoraproject.org/pub/fedora/linux/releases/26/Docker/x86_64/images/Fedora-Container-Minimal-Base-26-1.5.x86_64.tar.xz | docker load

Then replace the following line in _integrationtest/undertow/src/main/docker/Dockerfile_:

    FROM centos:7

with

    FROM fedora-container-minimal-base-26-1.5.x86_64

This results in a smaller size of the Undertow example Docker image.

## License

This project is licensed under the Apache License version 2.0.
