# Using `ko` with `Drone CI` and `k3d`

An example to show how to use [ko](https://ko.build) as part of [Drone CI](https://drone.io) pipelines to deploy an application to [k3s](https://k3s.io) cluster on your laptop.

## Pre-requisites

- [Docker for Desktop](https://www.docker.com/products/docker-desktop/)
- [Drone CLI](https://docs.drone.io/cli/install/)
- [k3d](https://k3d.io)
- [ko](https://ko.build)

## Run Pipeline

```shell
drone exec --trusted
```

If all goes well you should have the `hello-world` deployment running in your cluster.

Validate the deployment

```shell
export KUBECONFIG="$PWD/.kube/config.external"
```

Check the pods and services,

```shell
kubectl get pods,svc
```

```shell
NAME                               READY   STATUS    RESTARTS   AGE
pod/hello-world-566b6d7896-6qts9   1/1     Running   0          4m9s

NAME                  TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
service/kubernetes    ClusterIP   10.43.0.1      <none>        443/TCP    13m
service/hello-world   ClusterIP   10.43.224.48   <none>        8080/TCP   12m
```

Do port forward the `hello-world` service,

```shell
kubectl port-forward deployments/hello-world 8080
```

Calling the service,

```shell
curl localhost:8080/
```

## Using `ko` from host

If you have installed `ko` locally, then run the command to deploy the application.

```shell
export KUBECONFIG="$PWD/.kube/config.external"
export KO_DOCKER_REPO="k3d-myregistry.localhost:5001/examples"
ko apply --insecure-registry -f config/
```

## Clean up

```shell
drone exec --trusted pipeline=clean
```
