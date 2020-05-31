





Stream Processing Components

# Architecture Components

- Spring Data Flow Server 
    * Parses, Validates and Persist streams
    * Delegates deployments of stream to skipper
    * Registers source, transfomer and sink applications
- Spring Skipper Server
    * Manages streams deployment, upgrades and rolling back(
    In case of kubernetes as runtime platform. Deploys stream as pods)
- MySQL Database
    * Used by both the servers to store the state info about application registered, stream definition. 
- Message Broker Middleware(Rabbit MQ)
    * To send and receive the stream data/messages

## Additional compoments for Stream Monitoring 
- Prometheus 
    * Pull based time series data base to aggregate time series data in realtime
- Grafana
    * Visualize the TSD

# Infrastructure

This section provides a summary of run time platform for spring cloud data flow.

- Target Runtime environment: 
    * Kubernetes (minikube cluster locally)
    * Local

![Kubernetes Deployment](/design/images/Kubernetes deployment of spring cloud dataflow platform.png?raw=true)

# Application

Source (http) -> Transform -> Sink(log)