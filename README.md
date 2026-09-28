# Headscale Compose
My personal Headscale docker compose setup incl. a Headplane container as web UI and an external Traefik instance for serving them.

## Traefik Reverse Proxy
For as how to setup the Traefik refer to this [repository](https://github.com/saiba-tenpura/traefik-compose).

## Setup
Copy the .env.example file, set the passwords and adjust the variables to match your environment.
```bash
cp .env.example .env
```

Copy the example configuration files for Headscale and Headplane and adjust them to your needs.
```
cp headscale/config.example.yaml headscale/config.yaml
cp headplane/config.example.yaml headplane/config.yaml
```

## Custom DNS
In order for rexample to use your own private [Pi-hole](https://github.com/saiba-tenpura/docker-pi-hole) as DNS you first have to add it to your Tailnet.
```
# The Pi-hole is the DNS server itself so we don't need to accept it
sudo tailscale up --login-server https://headscale.example.com --accept-dns=false

# Note the IPs
tailscale ip
```

Then configure the DNS either in the headscale/config.yaml directly or via the Headplane UI and restart the containers.
```
...

dns:
  ...

  override_local_dns: true
  nameservers:
    global:
      - <DNS_TAILSCALE_IPv4>
      - <DNS_TAILSCALE_IPv6>
```

## Configuring OIDC
In order to to configure OIDC you first need an identity provider (IdP) e.g. [Authentik](https://github.com/saiba-tenpura/docker-authentik).

### Headscale
To configure OIDC for Headscale fill in the following in the headscale/config.yaml:
```yaml
server_url: https://headscale.example.com

...

oidc:
  issuer: "https://authentik.example.com/application/o/<HEADSCALE_SLUG>/"
  client_id: "<HEADSCALE_CLIENT_ID>"
  client_secret: "<HEADSCALE_CLIENT_SECRET>"
  pkce:
    enabled true

```

### Headplane
To configure OIDC for Headplane fill in the following in the headplane/config.yaml:
```yaml
server:
  base_url: https://headscale.example.com

...

headscale:

  ...

  api_key: "<HEADSCALE_API_KEY>"


oidc:
  issuer: "https://authentik.example.com/application/o/<HEADPLANE_SLUG>/"
  client_id: "<HEADPLANE_CLIENT_ID>"
  client_secret: "<HEADPLANE_CLIENT_SECRET>"
  use_pkce: true
```

For additional information refer to the respective documentations [Headscale](https://headscale.net), [Headplane](https://headplane.net).

## License
[MIT](./LICENSE)
