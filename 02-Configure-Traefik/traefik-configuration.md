# Configure Traefik

<img src="../img/Traefik_training.png" alt="Traefik Logo" height="350"> 


## Static Configuration using File configuration
1. Open you Terminal window and change to the `02-Traefik-Overview` folder
2. Open the `docker-compose.file.yml` file in your favorite editor and review how Docker starts Traefik using the file configuration
3. From the `02-Traefik-Overview` directory execute this command -> `docker-compose -f docker-compose.file.yml up -d`
4. Review the logs output `docker-compose logs`
5. Stop and clean-up `docker-compose stop`

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


## Static Configuration using CLI configuration
1. Open you Terminal window and change to the `02-Traefik-Overview` folder
2. Open the `docker-compose.cli.yml` file in your favorite editor and review how Docker starts Traefik using the CLI configuration
3. From the `02-Traefik-Overview` directory execute this command -> `docker-compose -f docker-compose.cli.yml up -d`
4. Review the logs output `docker-compose logs`
5. Stop and clean-up `docker-compose stop`

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

## Static Configuration using Environment variables configuration
1. Open you Terminal window and change to the `02-Traefik-Overview` folder
2. Open the `docker-compose.file.yml` file in your favorite editor and review how Docker starts Traefik using the Environment configuration
3. From the `02-Traefik-Overview` directory execute this command -> `docker-compose -f docker-compose.file.yml up -d`
4. Review the logs output `docker-compose logs`
5. Stop and clean-up `docker-compose stop`

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