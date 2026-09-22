# Headsacle Compose
My personal Headscale docker compose setup incl. an external Traefik instance for serving the web interface.

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

For additional information refer to the the respective documentations [Headscale](https://headscale.net), .[Headplane](https://headplane.net)

## License
[MIT](./LICENSE)
