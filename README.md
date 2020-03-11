# spring-boot-gke
A simple example of using Google Kubernetes Engine with a Spring Boot MicroService

http://localhost/8080/echo?message=test

# Run Standalone
```
mvn clean spring-boot:run
```

# Run In Docker
``` bash
 docker image build --tag="spring-boot/echo" .
 docker run -p 8080:8080 -t "spring-boot/echo"    
```

