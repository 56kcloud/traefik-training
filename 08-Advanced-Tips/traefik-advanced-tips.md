# Traefik Advanced Tips

<img src="../img/Traefik_training.png" alt="Traefik Logo" height="350"> 

## 1. Enable Traefik Pilot
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




# Continue to the Next to Advance Tips

### Click here to continue -> [Advanced Tips](https://github.com/56kcloud/traefik-training/blob/master/08-Advanced-Tips/traefik-advanced-tips.md)
