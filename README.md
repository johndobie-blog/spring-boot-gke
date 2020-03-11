# spring-boot-gke
A simple example of using Google Kubernetes Engine with a Spring Boot MicroService

http://localhost/8080/echo?message=test

# Run Standalone
```
mvn clean spring-boot:run
```

# Run In Docker Locally
``` bash
 docker image build --tag="spring-boot/echo" .
 docker run -p 8080:8080 -t "spring-boot/echo"    
```

# Run In Google Shell



https://console.cloud.google.com/cloudshell/open?git_repo=https://github.com/johndobie-blog/spring-boot-gke.git&page=editor&open_in_editor=README.md