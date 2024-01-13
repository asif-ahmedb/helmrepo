# helmrepo:
# Description

The project contains the configuration files for two micro-services namely airports-assembly and countries-assembly. An nginx container is used as reverse proxy to forward traffic to specific endpoints of the micro-services.
```
├── Chart.yaml
├── README.md
├── charts
│   ├── airports
│   │   ├── Chart.yaml
│   │   ├── charts
│   │   ├── templates
│   │   │   ├── airports-deployment.yaml
│   │   │   └── airports-service.yaml
│   │   └── values.yaml
│   ├── countries
│   │   ├── Chart.yaml
│   │   ├── charts
│   │   ├── templates
│   │   │   ├── countries-deployment.yaml
│   │   │   └── countries-service.yaml
│   │   └── values.yaml
│   └── nginx
│       ├── Chart.yaml
│       ├── templates
│       │   ├── nginx-deployment.yaml
│       │   └── nginx-service.yaml
│       └── values.yaml
└── values.yaml

```
**More details of the application and the GitHub repository is here: [https://github.com/asif-ahmedb/app-repo/tree/main](https://github.com/asif-ahmedb/app-repo/tree/main)**


**Prerequisites**
- Kubernetes [Minikube for deployment on user's machine]
- kubectl 
- Docker
- Helm

**Steps to Run(locally)**
- Clone the repo
- cd into the repo directory and run the helm commands
```
  cd helmrepo
  helm upgrade --install <RELEASE_NAME> . -n <NAMESPACE>
```
- Use kubectl commands to check if the services are up and running.[NOTE: On minikube, the services might take a little time to come up; specifically, the airports and countries deployments.]
- To check the version update features, modify the necessary values.yaml to update the version[docker image tag - from 1.0.1 to 1.1.0] and use helm to do an upgrade install. For now, this is for airports deployment **only**.

**On GitHub via GitHub Actions**

**Pre-requisites**: As the project uses EKS cluster, we need to provide the required AWS secrets and region details as repository secrets. Below are the details required:
    
    - AWS_ACCESS_KEY_ID
    - AWS_SECRET_ACCESS_KEY
    - AWS_REGION
    - K8S_CLUSTER [The name of the cluster where you want GitHub Actions to deploy]

Since the project uses a public docker repository for the images, we do not need any credentials for authentication. Alternatively, we can also use ECR(Elastic Container Registry) for pushing our container images and the above AWS secrets should be sufficient to pull/push images as long as we have the necessary access provided.

**GitHub Action in Action:**
- When we push code to the "main" branch, GitHub action kicks in and deploys the services to the kubernetes cluster. `This project uses an EKS cluster in a specific region to deploy the services from GitHub`.
- Once the job completes in GitHub, we can verify the deployments, pods, services on AWS EKS console or via command-line using kubectl.
