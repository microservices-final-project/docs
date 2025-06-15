# Step by Step of Project Proposal Solution

## Microservices

Fork to https://github.com/SelimHorri/ecommerce-microservice-backend-app

- Optimize DockerFiles

- Make microservices independent (this es necessary for to have repository for each microservice)
  - Define parent pom.xml dependences in each pom.xml microservice
  - In ApiGateway microservice change allowed_origins attribute for allowed-origins-pattern and add discovery config 
    ![changed application.yml config ](images/step_by_step/application_yml_api_gateway.png) 

Review maked changes in https://github.com/Alejolonber25/ecommerce-microservice-backend-app

Now you can build and push each microservice in your hub repository preferred and pass to config you can register the manifests in a cluster (in minikube or kubernetes by cloud services) (infra repository - k8s directory)

## Manifests


