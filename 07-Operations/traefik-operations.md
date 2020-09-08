# Traefik Operations Lab

<img src="../img/Traefik_training.png" alt="Traefik Logo" height="350"> 

## 1. Enable Healthcheck with CLI & Ping, Docker HEALTHCHECK, and Secure Dashboard/API
1. Before we begin, lets cleanup any running Docker stack `docker stack rm traefik` If you named you stack something else use your specified name. If you don't remember run `docker stack ls`
2. Change to the `07-Operations` folder
3. Open the `traefik.yml` file in your favorite editor and review the `Traefik API, Dashboard & Ping sections` section
4. In the API section Change `insecure` from `true` to `false` which is default

```yml
api:
  # Dashboard
  # true = default
  # 
  dashboard: true
  #
  # insecure: false is default
  #
  insecure: false
```

5. Review the docker `HEALTHCHECK` in `docker-compose.cli.yml`

```yml
healthcheck:
      test: ["CMD", "traefik", "healthcheck"]
      interval: 10s
      timeout: 2s
      retries: 3
      start_period: 5s
```

6.  Start Traefik with Ping endpoint, Secure Mode ,and Docker `HEALTHCHECK ` -> `docker stack deploy -c docker-compose.cli.yml traefik`
7.  Run `docker ps` notice the traefik container status now has Health status
8.  Open the Ping endpoint [http://your_domain_here/ping](http://traefik.localhost/ping)
9.  Make an API request [http://your_domain_here:8080/api/http/routers](http://traefik.localhost:8080/api/http/routers)
10. Open the Dashboard. Ensure to have the trailing "/" [http://your_domain_here/dashboard/](http://traefik.localhost/dashboard/)
11. The full list of API requests can be found here [https://docs.traefik.io/operations/api/](https://docs.traefik.io/operations/api/)


# Continue to the Next to Advance Tips

### Click here to continue -> [Advanced Tips](https://github.com/56kcloud/traefik-training/blob/master/08-Advanced-Tips/traefik-advanced-tips.md)
