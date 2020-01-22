# micro-k8s-example

## Setup and Dependencies

Start minikube

```shell
minikube start -p micro
```

Deploy etcd-operator and nats-operator on cluster

```shell
helm repo update
helm install etcd-operator stable/etcd-operator --set customResources.createEtcdClusterCRD=true

kubectl apply -f https://github.com/nats-io/nats-operator/releases/latest/download/00-prereqs.yaml
kubectl apply -f https://github.com/nats-io/nats-operator/releases/latest/download/10-deployment.yaml

kubectl apply -f https://raw.githubusercontent.com/micro/micro/master/platform/kubernetes/resource/nats/nats.yaml

```

Deploy micro-web

```shell
kubectl apply -f ./infrastructure/micro-web.yaml
```

Access micro-web

```shell
kubectl expose deployment micro-web --type=NodePort --name=micro-web #
kubectl describe services micro-web # Note NodePort
kubectl cluster-info # Note master ip
open http://<masterIP>:<nodePort>
```

## Deploy greeter example

```shell
kubectl apply -f ./infrastructure/deployment.yaml
```

## Test greeter with micro-web

Response:

```json
{
  "id": "go.micro.client",
  "code": 500,
  "detail": "Bad Request: HTTP status code 400; transport: received the unexpected content-type \"text/plain; charset=utf-8\"",
  "status": "Internal Server Error"
}
```
