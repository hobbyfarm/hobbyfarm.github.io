+++
title = "Backend Development"
+++

HobbyFarm makes use of a backend service called [Gargantua](https://github.com/hobbyfarm/gargantua). Gargantua is a RESTful API written in [Go](https://golang.org/). The following instructions will assist in setting up a local development environment for Gargantua.

The following instructions will assist in setting up a local development environment for Gargantua.

## Requirements

Gargantua is written in [Go](https://golang.org/). To develop Gargantua, knowledge of Go is required.

The following tools are required to be installed:

* [Docker](https://docs.docker.com/get-docker/)
* [Docker Compose Plugin](https://docs.docker.com/compose/install/linux/)


## Setup

The following instructions will assist in setting up a local development environment for Gargantua.

1. Clone the Gargantua repository.
    ```bash
    ## Clone the Gargantua repository
    git clone https://github.com/hobbyfarm/gargantua.git
    ```

2. Clone the HobbyFarm repository.
    ```bash
    ## Clone the HobbyFarm repository
    git clone https://github.com/hobbyfarm/hobbyfarm.git
    ```

3. Change into the HobbyFarm directory.
    ```bash
    ## Change into the Gargantua directory
    cd hobbyfarm
    ```

4. Modify the Docker Compose variables for the local environment as needed.
    ```bash
    ## Copy the example environment file
    cp .env.example .env

    ## Edit the file
    vim .env
    ```

5. Start the Docker Compose stack. This will provide a [K3d](https://k3d.io) cluster for [CustomResourceDefinitions (CRD)](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/).
    ```bash
    ## Start the Docker Compose stack
    ./compose.sh up

    ## Alternatively, build changes to the local development container
    ## Only required if a file in ./cicd/docker-local has changed
    ./compose.sh up --build

    ## To stop the Docker Compose stack
    ./compose.sh stop

    ## To destroy the Docker Compose stack
    ./compose.sh destroy
    ```

## Compose.sh Script

The `compose.sh` script is a wrapper around `docker-compose` that performs the following fuctions:
* Connects to the external Docker network called `hobbyfarm-dev`.
* Mounts the external volumes for [Kubernetes Service Account](https://kubernetes.io/docs/concepts/security/service-accounts/) credentials called `hobbyfarm-kube-sa`.
* Calls `docker-compose up` which starts the `hf-garg` container, listening on `localhost:16210`.
* The `hf-garg` container runs a watch loop on Golang files, rebuilds on detected changes.