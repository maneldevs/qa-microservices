# DEPLOY PRODUCTION USING DOCKER

## Create docker image

- Create microservice image using Google Jib Maven plugin
```
cd user-service
./mvnw jib:dockerBuild
```

- Verify image is created
```
docker images | grep qa
```

- Move images to production server
```
# in local device
docker save -o <path for generated tar file> <image name>
# copy your image to production server. In production server
docker load -i <path to image tar file>
```

## Run a service from terminal

- Start only a service in detach mode
```
docker run -d -p8080:8080 -e "SPRING_PROFILES_ACTIVE=docker" --name=qa-user-service qa/user-service:1.0.0
```

- Verify container runs properly
```
curl http://localhost:8080/actuator/health
```

- Get all logs and wait for new ones
```
docker logs qa-user-service -f
```

- Wait for new logs
```
docker logs qa-user-service -f --tail 0
```

- Stop and delete the container
```
docker rm -f qa-user-service
```

- Verify running containers
```
docker ps
```

## Run all services with docker-compose

- Every change in code in a microservice requires build the image
```
cd user-service
./mvnw jib:dockerBuild
```

- Start all services
```
cd ../.docker-prod
docker-compose up -d
```

- Get all logs and wait for new ones
```
docker-compose logs -f
```

- Get all logs and wait for new ones only for a container
```
docker-compose logs -f qa-user-service
```

- Wait for new logs
```
docker-compose logs -f --tail 0
```

- Verify endpoint
```
curl http://localhost:8080/qa-composite/questions/281a6c13-56b5-4c12-84ce-f12bfa3e1bc3 | jq
```

- Stop and remove all
```
docker-compose down
```
