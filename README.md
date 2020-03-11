# spring-boot-gke
A simple example of using Google Kubernetes Engine with a Spring Boot MicroService

http://localhost/8080/echo?message=test

### Run Standalone
```
mvn clean spring-boot:run
```

### Run In Docker Locally
``` 
 docker image build --tag="spring-boot/echo" .
 docker run -p 8080:8080 -t "spring-boot/echo"    
```

### Run In Google Shell
```
https://console.cloud.google.com/cloudshell/open?git_repo=https://github.com/johndobie-blog/spring-boot-gke.git&page=editor&open_in_editor=README.md
mvn clean spring-boot:run
```

### Run In Google Shell with Docker
https://console.cloud.google.com/cloudshell/open?git_repo=https://github.com/johndobie-blog/spring-boot-gke.git&page=editor&open_in_editor=README.md

```
mvn clean install
docker image build --tag="spring-boot/echo" .
docker run -p 8080:8080 -t "spring-boot/echo"    
```

### Run with Kubernetes

#### Open google shell
```
https://console.cloud.google.com/cloudshell/open?git_repo=https://github.com/johndobie-blog/spring-boot-gke.git&page=editor&open_in_editor=README.md
```

#### Create and set the project
```
gcloud projects create spring-boot-kubernetes-echo --name="Spring Boot Echo on GKE"
gcloud config set project spring-boot-kubernetes-echo

export PROJECT_ID=”$(gcloud config get-value project -q)”

gcloud projects list
gcloud beta billing projects link spring-boot-kubernetes-echo --billing-account 003146-4D6988-3DC95C
```

#### Create the docker image
``` 
gcloud services enable containerregistry.googleapis.com

docker build -t gcr.io/spring-boot-kubernetes-echo/echo .
docker push gcr.io/spring-boot-kubernetes-echo/echo
```

#### Create the kubernetes cluster
```
gcloud services enable compute.googleapis.com container.googleapis.com

gcloud container clusters create echo-kubernetes-cluster \
  --num-nodes 2 \
  --machine-type n1-standard-1 \
  --zone us-central1-c
   
kubectl create deployment echo-deployment --image=gcr.io/spring-boot-kubernetes-echo/echo:latest
kubectl get deployments 
kubectl get pods
```

#### Create a loadbalancer and expose the service
```
kubectl expose deployment echo-deployment --type=LoadBalancer --port 80 --target-port 8080

kubectl get svc
NAME              TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)        AGE
echo-deployment   LoadBalancer   10.59.247.132   35.222.75.46   80:31383/TCP   80s
```

Use the external address to access the service http://35.222.75.46/echo?message=test 

#### Scale the pods up or down
```
kubectl scale deployment image --replicas=2
kubectl get pods
kubectl scale deployment image --replicas=1
kubectl get pods
```

##### Tear Down.
```
gcloud projects delete spring-boot-kubernetes-echo -q
```
