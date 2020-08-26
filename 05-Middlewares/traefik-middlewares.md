# Traefik Middlewares Lab

<img src="../img/Traefik_training.png" alt="Traefik Logo" height="350"> 

## 1. Add Basic Authentication to our CatApp
1. Before we begin, lets cleanup any running Docker stack `docker stack rm traefik` If you named you stack something else use your specified name. If you don't remember run `docker stack ls`
2. Change to the `05-Middlewares` folder
3. Generate a new password for our `catapp` by running `echo $(htpasswd -nb traefik training) | sed -e s/\\$/\\$\\$/g`
4. Every `$` in the password needs to have double `$$` to escape the characters correctly. 

Run the `htpasswd` command

```bash
echo $(htpasswd -nb traefik training) | sed -e s/\\$/\\$\\$/g
traefik:$apr1$.zPbdVg8$LcHeyCZElH.JfxkxxlMPI.

```

5. Open the `docker-compose.auth.yml` file in your favorite editor and review the `catapp` section
6. Edit the `docker-compose.auth.yml` `catapp` section and add the Auth Middleware to our `catapp` router  `- "traefik.http.middlewares.test-auth.basicauth.users=traefik:$$apr1$$.zPbdVg8$$LcHeyCZElH.JfxkxxlMPI."`
7. Update the **Router** to point to the new Middleware `- "traefik.http.routers.catapp.middlewares=test-auth"`
8. Start Traefik and the `catapp` `docker stack deploy -c docker-compose.auth.yml traefik`
9. Open the Traefik Dashboard [http://0.0.0.0:8080](http://0.0.0.0:8080) and verify the new `test-auth` **Middleware** is running and and assigned to the `catapp` service
10. Open the `catapp` application in a new browser tab [http://catapp.localhost](http://catapp.localhost)
11. Enter the user `traefik` and password `training` to visit your `catapp` application

## 2. Add Compression Middleware to our CatApp
1. Before we begin, lets cleanup the HTTP stack  `docker stack rm traefik` If you named you stack something else use your specified name. If you don't remember run `docker stack ls`
2. Change to the `05-HTTPS-and-TLS` folder
3. Open the `docker-compose.compress.yml` file in your favorite editor and review the `catapp` section
4. Add the **Compress Middleware** to our `catapp` section `- "traefik.http.middlewares.test-compress.compress=true"`
5. Update the router to include the **Compress Middleware** ` - "traefik.http.routers.catapp.middlewares=test-auth,test-compress"`
6. Start Traefik and the `catapp` `docker stack deploy -c docker-compose.compress.yml traefik`
7. Open the Traefik Dashboard [http://0.0.0.0:8080](http://0.0.0.0:8080) and verify the new `test-compress` **Middleware** is running and and assigned to the `catapp` service
8.  Open the `catapp` application in a new browser tab [http://catapp.localhost](http://catapp.localhost)

## 3. Add Error Pages Middleware
1. Before we begin, lets cleanup the HTTP stack  `docker stack rm traefik` If you named you stack something else use your specified name. If you don't remember run `docker stack ls`
2. Change to the `05-HTTPS-and-TLS` folder
3. Open the `docker-compose.error.yml` file in your favorite editor and review the `catapp` section
4. Add the Error Page service below the `catapp`

  ```yaml
  # Error Page service
    error:
      image: guillaumebriday/traefik-custom-error-pages
      labels:
            - "traefik.enable=true"
            - "traefik.http.routers.error.rule=Host(`error.localhost`)"
            - "traefik.http.routers.error.service=error"
            - "traefik.http.services.error.loadbalancer.server.port=80"
            - "traefik.http.routers.error.entrypoints=web"
  ```

4. Add the **Error Page Middleware** to our `catapp` section

```yaml
# Error Pages Middleware
       - "traefik.http.middlewares.test-errorpages.errors.status=400-599"
       - "traefik.http.middlewares.test-errorpages.errors.service=error"
       - "traefik.http.middlewares.test-errorpages.errors.query=/{status}.html"
```


5. Update the router to include the **Error Page middleware** ` - "traefik.http.routers.catapp.middlewares=test-auth,test-compress,test-errorpages"`
6. Start Traefik and the `catapp` `docker stack deploy -c docker-compose.error.yml traefik`
7. Open the Traefik Dashboard [http://0.0.0.0:8080](http://0.0.0.0:8080) and verify the new `test-errorpages` **Middleware** is running and and assigned to the `catapp` service
8.  Open the `catapp` application in a new browser tab [http://catapp.localhost](http://catapp.localhost)
9.  Let's produce a 404 error to see our error page in actions. Open a new browser tab [http://catapp.localhost/broken](http://catapp.localhost/broken)

## 4. Add Rate Limit Middleware
1. Before we begin, lets cleanup the HTTP stack  `docker stack rm traefik` If you named you stack something else use your specified name. If you don't remember run `docker stack ls`
2. Change to the `05-HTTPS-and-TLS` folder
3. Open the `docker-compose.ratelimit.yml` file in your favorite editor and review the `catapp` section
4. Add the **Rate Limit Middleware** to our `catapp` section `- "traefik.http.middlewares.test-ratelimit.ratelimit.average=2"`
5. Update the router to include the **Rate Limit middleware** ` - "traefik.http.routers.catapp.middlewares=test-auth,test-compress,test-ratelimit"`
6. Start Traefik and the `catapp` `docker stack deploy -c docker-compose.ratelimit.yml traefik`
7. Open the Traefik Dashboard [http://0.0.0.0:8080](http://0.0.0.0:8080) and verify the new `test-ratelimit` **Middleware** is running and and assigned to the `catapp` service
8. Open the `catapp` application in a new browser tab [http://catapp.localhost](http://catapp.localhost)
9. Refresh the `catapp` page quickly to see the Rate Limit error

## 5. Add Redirect Middleware

**This section we need your DNS settings again from Section `04-HTTPS-TLS` We will use the DNS settings to test the HTTP -> HTTPS redirect. Ensure your DNS settings are configured in the `docker-compose.redirect.yml`**

1. Before we begin, lets cleanup the HTTP stack  `docker stack rm traefik` If you named you stack something else use your specified name. If you don't remember run `docker stack ls`
2. Change to the `05-HTTPS-and-TLS` folder
3. Open the `docker-compose.redirect.yml` file in your favorite editor and review the `catapp` section
4. Add the **Redirect Scheme middleware** to our `catapp` section

  ```yaml
        - "traefik.http.middlewares.test-redirectscheme.redirectscheme.scheme=https"
        - "traefik.http.middlewares.test-redirectscheme.redirectscheme.permanent=true"
  ```


4. Update the router to include the **Redirect Scheme middleware** ` - "traefik.http.routers.catapp.middlewares=test-auth,test-compress,test-redirectscheme"`
5. Update your domain name in `- "traefik.http.routers.catapp.rule=Host(`<your-domain-here>`)"`
6. Add your DNS tokens to the Enviornment section of Traefik
7. Start Traefik and the `catapp` `docker stack deploy -c docker-compose.redirect.yml traefik`
8. Open the Traefik Dashboard [http://0.0.0.0:8080](http://0.0.0.0:8080) and verify the new `test-redirectscheme` **Middleware** is running and and assigned to the `catapp` service
9.  Open the `catapp` application in a new browser tab and open `your-domain` configured in the DNS section
10. You should see your `catapp` domain redirect from HTTP -> HTTPS automagically. 

# Continue to the Next Observability

### Click here to continue -> [Observability Lab](https://github.com/56kcloud/traefik-training/blob/master/06-observability/traefik-observability.md)
