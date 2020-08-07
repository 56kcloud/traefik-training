# Configure Traefik

<img src="../img/Traefik_training.png" alt="Traefik Logo" height="350"> 


## 1. Static Configuration using File configuration
1. Clean up previous Lab. Open you Terminal window and change to the `01-Traefik-Overview` directory. Stop the previous section Lab `docker-compose stop`
2. Change to the `02-Configure-Traefik` folder
3. Open the `docker-compose.file.yml` file in your favorite editor and review how Docker starts Traefik using the file configuration
4. From the `02-Traefik-Overview` directory execute this command -> `docker-compose -f docker-compose.file.yml up -d`
5. Review the logs output `docker-compose -f docker-compose.file.yml logs`
6. Stop and clean-up `docker-compose -f docker-compose.file.yml stop`

### File static config code snippet

```yaml
################################################################
# API and dashboard configuration
################################################################
api:
  # Dashboard
  #
  #
  dashboard: true
################################################################
# Docker configuration backend
################################################################
providers:
  docker: 
    watch: false
    exposedByDefault: false
    swarmMode: true
################################################################
# Traefik Logging
################################################################
log:
  level: INFO
```


## 2. Static Configuration using CLI configuration
1. Open the `docker-compose.cli.yml` file in your favorite editor and review how Docker starts Traefik using the CLI configuration
2. From the `02-Configure-Traefik` directory execute this command -> `docker-compose -f docker-compose.cli.yml up -d`
3. Review the logs output `docker-compose -f docker-compose.cli.yml logs`
4. Stop and clean-up `docker-compose -f docker-compose.cli.yml stop`

### CLI static config code snippet

```yml
services:
  traefik:
    # The latest official supported Traefik docker image
    image: traefik:v2.3
    # Enables the Traefik Dashboard and tells Traefik to listen to docker
    # --providers tell Traefik to connect to the Docker provider
    # enable --log.level=INFO so we can see what Traefik is doing in the log files
    command: 
      - "--api.insecure=true"
      - "--providers.docker" 
      - "--log.level=INFO"
```

## 3. Static Configuration using Environment variables configuration
1. Open the `docker-compose.env.yml` file in your favorite editor and review how Docker starts Traefik using the Environment configuration
2. From the `02-Configure-Traefik` directory execute this command -> `docker-compose -f docker-compose.env.yml up -d`
3. Review the logs output `docker-compose -f docker-compose.env.yml logs`
4. Stop and clean-up `docker-compose -f docker-compose.env.yml stop`

### Environment variables static config code snippet
```yml
services:
  traefik:
    # The latest official supported Traefik docker image
    image: traefik:v2.3
    # Enables the Traefik Dashboard and tells Traefik to listen to docker
    # --providers tell Traefik to connect to the Docker provider
    # enable --log.level=INFO so we can see what Traefik is doing in the log files
    environment:
      - TRAEFIK_API_INSECURE=true
      - TRAEFIK_PROVIDERS_DOCKER=true
      - TRAEFIK_LOG_LEVEL=INFO
```

## 4. Static Configuration Entrypoint Lab
In this section, we will start using Docker Swarm. Ensure you have followed the Prerequisites from the Lab Setup. This can be found in the Getting Started Section of the course if you need to revisit. If you have any issues please add a comment in the course.

## NOTE about Docker ##
We use the `docker stack deploy -c <compose file name> <user provided stack name>` command to deploy Swarm services. `docker stack ps <stack name>` is used to check if all services are running in the stack. Finally, `docker stack rm <stack name>` removes the entire stack. 

To check logs on a running service, `docker service ls` to retrieve the name of the service and `docker service logs <service name>` to vier it's logs. 

Further Docker information can be found here for [Docker Stack](https://docs.docker.com/engine/reference/commandline/stack/) or [Docker services](https://docs.docker.com/engine/reference/commandline/service/)

1. Open the `traefik-entrypoints.yml` and `docker-compose.configuration.yml` files in your favorite editor and review how Entrypoints are created and how we are using the YML file in this Lab.
2. From the `02-Configure-Traefik` directory execute this command -> `docker stack deploy -c docker-compose.configuration.yml catapp`
3. Open the Traefik Dashboard [http://0.0.0.0:8080](http://0.0.0.0:8080) and review how the test service `catapp` is configured with **Entrypoints**.
4. Review the `catapp@docker` router and `catapp@docker` service in the Traefik Dashboard
5. Test the newly deployed `catapp` service [http://catapp.localhost/](http://catapp.localhost/)
6. Stop and clean-up `docker stack rm catapp`


# Continue to the Next Lab Routers & Services

### Click here to continue -> [Routers & Services Lab](https://github.com/56kcloud/traefik-training/blob/master/03-Routers-and-Services/traefik-routers-and-services.md)