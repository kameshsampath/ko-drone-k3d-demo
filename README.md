# Using `ko` with `Drone CI` and `k3d`

An example to show how to use [ko](https://ko.build) as part of [Drone CI](https://drone.io) pipelines to deploy an application to [k3s](https://k3s.io) cluster on your laptop.

## Pre-requisites

- [Docker for Desktop](https://www.docker.com/products/docker-desktop/)
- [Drone CI](https://drone.io)
- [k3d](https://k3d.io)
- [ko](https://ko.build)

## Run Pipeline

```shell
drone exec --trusted
```

## Deploy from Host

Run the same command from the host directly which succeeds,

```shell
export KUBECONFIG="$PWD/.kube/config.external"
export KO_DOCKER_REPO="k3d-myregistry.localhost:5001/examples"
ko apply --insecure-registry -f config/
```

## Clean up

```shell
drone exec --trusted pipeline=clean
```