## Notejam

Based on the monolithic application stated on the requested assignment, I chose to convert the architechture into one composed by microservices.

First of all, I chose to use Flask as I have previous experience working with this framework.

In order to renew this application I planned to develop it using Kubernetes which is one of the best solutions to manage a containerized application. Firstly I created  a docker image base from scratch  on the Notejam application using a Dockerfile `(javier/docker/Dockerfile)` which includes all the necessary steps to build it. After that I decided to use Helm as it is a very good option to manage Kubernetes applications.

Command for building the image from the root path of the repository: `docker build -t notejam:0.1.1 . -f ./javier/docker/Dockerfile`.

This Helm Chart is a basic implementation of what it would be in production, here you can find a deployment to ensure no interruption or downtime when an update is applied, a service and a hpa which adapts the deployment to serve a variable amount of traffic.

You can deploy the application using the following command from the root path of this repository: `helm install notejam javier/helm -f javier/helm/values.yaml`.

Use this command to connect to the application from your local machine:`kubectl port-forward svc/notejam 5000:5000`.

## ***Extra tools that I have used to deploy the application locally:***
- Minikube: this tool allows us to run Kubernetes locally as well as deploying the ingress-controller easily.
- Docker Registry: in where I push and pull the Nodejam docker image.

## ***Public cloud deployment (Azure):***
In case of deploying the application on Azure, I would have decided to use AKS as the cluster would be administered by Azure itself so it would leverage a lot of work. It would also guarantee us an uptime of 99.95% for the Kubernetes API Server for AKS Clusters that use Azure Availability Zone, and 99.9% for AKS Clusters that do not use Azure Availability Zone. For this same reason I would have had to create a PostgreSQL database managed by Azure instead of using a SQLite database inside the docker image, which is not a good practice. Using Azure Database for PostgreSQL, provides us a guaranteed high level of availability, and it also makes easier to take backups of the data and store them on Azure Backup allowing us to retain backups for up to 10 years. In case we wanted to have our backups always available we could choose the geo-redundant backup. Apart from that I would have had to store the created docker image using an instance of Azure Container registry. All those services would have been part of the same resource group.

## ***Extra tools or strategies that I would use:***
- Velero: for backing up and migrating Kubernetes resources.
- Ingress and Nginx: an ingress to access the application from outside and Nginx for the ingress controller.
- Git: to store our source code and as an essential part of our CI/CD cycle and for defining a branch strategy to deploy the cluster on different environments.
- Helm repository: using Azure Blob container.
- Jenkins: to automate our CI/CD flow. It will help us creating the docker image and pushing it to our repository, running the unit tests of the application,  and packaging and deploying the helm chart onto the specific environment.
- Namespace/Clusters separate the different environments using namespaces or clusters.
- Logging/Monitoring: Fluent Bit, ELK stack, Grafana, Prometheus. In addition to that: custom metrics to scale our application based on a specific metric.
- WSGI: use a production WSGI server to run the Flask application.