# OI4 OEC Onboarding

This Readme contains all the information to onboard an development target onto a basic OI4 Open Edge Computing without a real field device. Next to this manual it contains a docker compose file which already incorporates all the parts presented in this Instruction.

## Prerequisites
You need to **know** the basics on:
- how to work with a linux machine
- technologies like ssh and linux shells (e.g. bash)
- have a basic understanding of javascript/typescript projects and docker
- have Docker Desktop installed (when using Windows or MacOS as a development machine)

You need to **have** to following hardware available:
- Raspberry Pi 3B+ or above
- microSD card to boot the pi from (32GB or bigger)
- the possibility to access the internet with the pi -> check with your local IT department!
    - e.g. an router or
    - a desktop switch
- a network cable to connect the pi to the switch

Your need to **have access** to the [Open Industry 4.0 Alliance - Community](https://github.com/OI4) on GitHub. 
- Contact [Thomas Weinschenk](https://github.com/Weinschenk) if you need access.

## Overview

To onboard the development target, multiple different repositories from the OI4 Github Community are needed:

Repo Name | Repo Link |  Used for
--------- | --------- |  --------
OI4 OEC Registry | https://github.com/OI4/oi4-oec-registry | Registers oncoming oi4-compatible participants (Applications/Devices) and displays a basic check on the stats of the assets
OI4 OEC Service | https://github.com/OI4/oi4-oec-service | OI4-compliant base service 
OI4 OEC Demo | https://github.com/OI4/oi4-oec-demo | Monorepo containing several demos & tooling

The stack will also contain other tools:
Tool Name | Link |  Used for
--------- | --------- |  --------
Portainer CE | https://www.portainer.io/ | Container Deployment Management
Mosquitto | https://mosquitto.org/ | MQTT broker for the oi4 communication
Node-Red | https://nodered.org/ | Low-Code environment for easy access to debugging


## How to use

All the steps shown here are tested using an Raspberry Pi 3B+. Before starting with the steps below, setup your hardware deployment accordingly.

### Prepare the development target

The first step is to prepare the microSD card to boot. The most easy way is using the official [RaspberryPi OS Imager](https://www.raspberrypi.com/software/). Other ways if the imager is not available to you are also shown there.
- enable ssh in the extended settings and Wifi Config if needed
- (optional) set host name
- flash the image onto the microSD card following the instructions of the imager

After it has finished, insert the microSD card into your RaspberryPi and power it on. After it has booted, get the IP address of the pi and access it (e.g. using ssh). Check, if internet access works, as this is needed for the following steps.

### Install Docker

Docker is used to run all the OI4 applications as linux containers for easier deployment management. To install the docker runtime on the pi use the [official guide](https://docs.docker.com/engine/install/raspbian/).

### Install Portainer CE

From now on, all applications are deployed using docker. For the management of all containers and stacks on the RaspberryPi Portainer CE is used. To install it, use the [official documentation](https://docs.portainer.io/start/install-ce/server/docker/linux).

### Authenticating to the container registry

Before we start to create the stack for the OI4 demo deployment, we have to authenticate against the OI4 GitHub container registry to access the private images. Currently, this can be done using an access token. The [official guide](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#authenticating-to-the-container-registry) for the container registry contains a detailed description how to set it up and get going.

### Create the stack

To create the stack, some preparations have to be done. First, all the volumes for the containers have to be prepared beforehand. This can be easily done via the Portainer UI. Choose
> *Volumes* -> *+ Add volume*

and create the following volumes with the exact names:
- *node-red-data*
- *mosquitto-conf*
- *mosquitto-data*
- *mosquitto-logs*

After the creation of the volumes, use your personal access token to pull the latest version of the oi4-o3c-registry from the [container registry](https://github.com/OI4/oi4-oec-registry/pkgs/container/oi4-registry).

After the registry image has been pulled, choose 
> *Stacks* -> *+ Add stack*

in the Portainer UI and copy the contents of the *docker-compose.yml* file into the *Web-Editor* field. Click

> *Deploy stack*

To start the stack.

### Senda data via the message bus