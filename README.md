# weird.io.case-study

This repo contains the explaination for the design and architecture of application and platform. It includes all the components required to spin up the platform for spring boot data flow. 

# Setting up the platform 

## Deploying with Kubectl 

For kubernetes deployment you need to first create the cluster: 

We have successfully deployed cluster using [Minikube](https://kubernetes.io/docs/setup/learning-environment/minikube/).

## Start the minikube cluster 

When starting Minikube you should allocate some extra resources since we will be deploying several services. We have used following config to start minikube : 

```bash 
minikube start --cpus=4 --memory=10240 --vm-driver=virtualbox
```

## Installing and Setting up kubectl to interact with kubernetes 

[Here you can find the instruction to setup kubectl](
https://kubernetes.io/docs/user-guide/prereqs/)


# Setting up the platform 

1. Install the Message Broker

For applications to communicate with each other, you need to deploy the message broker.

RabbitMQ : Run the following command to start the RabbitMQ service:

```bash
kubectl apply -f kubernetes/rabbitmq/
```

2. Deploy MySQL : Run the following command to start the MySQL service:

```bash
kubectl apply -f kubernetes/mysql/
```

3. Deploy Prometheus and Grafana 
Prometheus is configured to scrape each proxy instance.Proxies in turn use the connection to pull metrics from each application. The scraped metrics are viewable through Grafana dashboards. 

Run the following commands to create the cluster role, binding, and service account:

```bash
kubectl apply -f kubernetes/prometheus/prometheus-clusterroles.yaml
kubectl apply -f kubernetes/prometheus/prometheus-clusterrolebinding.yaml
kubectl apply -f kubernetes/prometheus/prometheus-serviceaccount.yaml
```
Run the following commands to deploy Prometheus RSocket Proxy:

```bash
kubectl apply -f kubernetes/prometheus-proxy/
```
Run the following commands to deploy Prometheus:

```bash
kubectl apply -f src/kubernetes/prometheus/prometheus-configmap.yaml
kubectl apply -f src/kubernetes/prometheus/prometheus-deployment.yaml
kubectl apply -f src/kubernetes/prometheus/prometheus-service.yaml
```

Run the following command to deploy Grafana:

```bash
kubectl apply -f kubernetes/grafana/
```

Run the following command to get the grafana service endpoint: 

```bash
minikube service --url grafana
```

4. Deploy Data Flow Role Bindings and Service Account

```bash
kubectl apply -f kubernetes/server/server-roles.yaml
kubectl apply -f kubernetes/server/server-rolebinding.yaml
kubectl apply -f kubernetes/server/service-account.yaml
```

5. Deploy Skipper

Data Flow delegates the streams lifecycle management to Skipper. You need to deploy Skipper to enable the stream management features.

```bash
kubectl apply -f kubernetes/skipper/skipper-config-rabbit.yaml
kubectl apply -f kubernetes/skipper/skipper-deployment.yaml
kubectl apply -f kubernetes/skipper/skipper-svc.yaml
```

6. Deploy Data Flow Server

Before deployment change the granafa server url. 

```bash
kubectl apply -f kubernetes/server/server-config.yaml
kubectl apply -f kubernetes/server/server-svc.yaml
kubectl apply -f kubernetes/server/server-deployment.yaml
```

7. Get the data flow server url

```bash 
minikube service --url scdf-server
```