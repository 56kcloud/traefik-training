# Getting Started with Traefik

<img src="../img/Traefik_training.png" alt="Traefik Logo" height="350"> 


## 1. Starting Traefik for the first time
1. Open you Terminal window and change to the `01-Traefik-Overview` folder
2. Open the `docker-compose.yml` file in your favorite editor and review how Docker starts Traefik
3. From the `01-Traefik-Overview` directory execute this command -> `docker-compose up -d`
4. Review the logs output `docker-compose logs`

##  2. Connect a new service to Traefik
1. Uncomment the below whoami section inside the `docker-compose.yml`. Review the `whoami.yml` file for the complete solution.

```yaml
whoami:
     # A container that exposes an API to show its IP address
     image: containous/whoami
     # We set a label to tell Traefik to assign a hostname to the new service
     labels:
       - "traefik.http.routers.whoami.rule=Host(`whoami.docker.localhost`)"
```

2. Run `docker-compose up -d whoami`
3. Open a browser tab and paste `whoami.docker.localhost`  or from a terminal window `curl -H Host:whoami.docker.localhost http://127.0.0.1` and you should see the below results but with your IP addresses.

```yml
Hostname: 931789a57923
IP: 127.0.0.1
IP: 172.20.0.3
RemoteAddr: 172.20.0.2:46648
GET / HTTP/1.1
Host: whoami.docker.localhost
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.89 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Cache-Control: max-age=0
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Upgrade-Insecure-Requests: 1
X-Forwarded-For: 172.20.0.1
X-Forwarded-Host: whoami.docker.localhost
X-Forwarded-Port: 80
X-Forwarded-Proto: http
X-Forwarded-Server: 07d5bffe1678
X-Real-Ip: 172.20.0.1

```

##  3. Scale the Whoami service to 3x
1. Open you Terminal window and change to the `01-Traefik-Overview` folder
2. Let's scale the `whoami` service to 3x instances by typing `docker-compose up -d --scale whoami=3`
3. Open a browser tab and paste `whoami.docker.localhost`  or from a terminal window `curl -H Host:whoami.docker.localhost http://127.0.0.1` and you should see the 3rd IP address update based on which service is responding.

##  4. Get to know the Traefik Dashboard
1. Open a browser tab and type or click: http://0.0.0.0:8080 to open the Traefik Dashboard


# Continue to the Configure Traefik Lab

### Click here to continue -> [Configure Traefik](https://github.com/56kcloud/traefik-training/blob/master/02-Configure-Traefik/traefik-configuration.md)
