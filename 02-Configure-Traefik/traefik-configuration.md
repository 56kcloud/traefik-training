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

# 4. Static Configuration Entrypoint Lab
1. Open the `traefik-configuration.yml` file in your favorite editor and review how Entrypoints are used.
2. From the `02-Configure-Traefik` directory execute this command -> `docker-compose -f docker-compose.configuration.yml up -d`
3. Review the logs output `docker-compose -f docker-compose..yml logs`
4. Stop and clean-up `docker-compose -f docker-compose.env.yml stop`
