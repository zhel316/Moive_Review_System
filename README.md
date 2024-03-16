# Application Architecture
## UML Diagram for Database 
[IMDB 5000 Movie Dataset](https://www.kaggle.com/datasets/carolzhangdc/imdb-5000-movie-dataset?resource=download)
[UML Diagram](./images/UML.png)
## Interactive Components and Flow
[Data Flow in Web Application Architecture](./images/Flow.png)
# Deployment Environment
## Google Kubernetes Engine
- Our environment for running and deploying containers is GKE which provides the load balancer and SSL certificate.
- The storage class we used is standard-rwo which is a persistent disk that allows reading and writing once.
## Roles
We defined three roles or cluster roles to reduce security risks and configuration errors.
1. **Developer (Role):** Have authority to create, read, update, and delete Pods, Deployments, ConfigMaps, etc. for front and back-end services
2. **Authorization (ClusterRole):** Have broader access to cluster-wide monitoring, logging, updating, maintenance, etc.
3. **CI/CD Manager (ClusterRole):** Have authority to deploy and update applications, which may include access to resources such as Deployments, Services, ConfigMaps, and Secrets.
## Network Policy
We designed three network policies to limit **ingress** traffic:
1. Allow all ingress traffic to Web fronted pods
2. Allow all ingress traffic to API backend pods
3. Only all ingress traffic to database pods from Web-fronted pods and API backend pods.

# Operation Commands
## Docker Image Creation
Ensure you're in the `/Movies_Review_Web_App` directory before executing the commands below to build images.
1. Command for building fronted image.  
```
  cd film_frontend  
  docker buildx build --platform linux/amd64 -t liuzhe2022/filmcomments-web:latest --push .
```
2. Command for building backend image.  
```
   cd film_backend   
   docker buildx build --platform linux/amd64 -t liuzhe2022/filmcomments-api:latest --push .
```
## Deploy Instructions
Make sure you are in the `\Movies_Review_Web_App
### helm install
```
helm install app-release1 ./helmcharts 
```
### helm scaling
```
kubectl scale deployment app-release1-filmcomments-frontend --replicas=3
kubectl get deployments -n my-app
```
### helm uninstall
```
helm uninstall app-release1
```
### helm upgrade (default: rolling upgrade)
```
helm upgrade app-release1 ./helmcharts --set backend.api.container.image=liuzhe2022/filmcomments-api:test
```
## rollout
```
kubectl rollout status deployment/app-release1-filmcomments-api-deployment
```
