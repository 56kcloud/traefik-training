# Traefik Routers & Services

<img src="../img/Traefik_training.png" alt="Traefik Logo" height="350"> 


## 1. Deploying a Traefik Router, Service, and Load balancer
1. Before we begin, lets cleanup any running Docker stack `docker stack rm traefik` If you named you stack something else use your specified name. If you don't remember run `docker stack ls`
2. Change to the `03-Routers-and-Services` folder
3. Open the `docker-compose.yml` file in your favorite editor and review the `catapp` section
4. Start Traefik and the `catapp` but with no labels `docker stack deploy -c docker-compose.yml traefik`
5. ----- ######### -----
6. From the `03-Routers-and-Services` directory execute this command -> `docker-compose -f docker-compose.file.yml up -d`
7. Review the logs output `docker-compose -f docker-compose.file.yml logs`
8. Stop and clean-up `docker-compose -f docker-compose.file.yml stop`

### Service Labels for this section

```yaml
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.catapp.rule=Host(`catapp.localhost`)"
      - "traefik.http.routers.catapp.entrypoints=web"
      - "traefik.http.routers.catapp.service=catapp"
      - "traefik.http.services.catapp.loadbalancer.server.port=5000"
```


## 2. Troubleshooting Router / Service configurations
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