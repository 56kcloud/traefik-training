# Getting Started with Traefik

<img src="../../img/Traefik_training.png" alt="Traefik Logo" height="350"> 


## Starting Traefik for the first time
1. Open you Terminal window and change to the `01-Traefik-Overview` folder
2. Open the `docker-compose.yml` file in your favorite editor and review how Docker starts Traefik
3. From the `01-Traefik-Overview` directory execute this command -> `docker-compose up -d`
4. Review the logs output `docker-compose logs`

##  Connect a new service to Traefik
1. In the same directory is also the `whoami`service in the 
2. Launch the `whoami` service using `docker-compose` --> `docker-compose -f whoami.yml up -d`
3. Open a browser tab and paste `whoami.docker.localhost` 
