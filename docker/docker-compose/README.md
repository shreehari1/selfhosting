# Docker compose file collectiom

Docker compose files for quick reference

## Docker compose

### Docker compose installation

Install docker and docker compose with
```bash
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker $(whoami)
exit
```
Docker compose can be run with:
```
docker compose up -d
```
Compose file path can be specified with `-f /path/to/compose.yaml` in docker compose commands

Docker list
```
docker compose ps
```
Docker compose down with:
```
docker compose down
```
Docker compose stop with:
Specific container name can be passed to stop them. if left blank all will be stoped. 
```
docker compose stop <container>
```
Docker compose restart with:
Specific container name can be passed to restart them. if left blank all will be restarted. 
```
docker compose restart <container>
```

### Docker network

Create docker network with: 
```
docker network create <docker_network>
```
inspect network with:
```
docker network inspect <docker_network>
```
Docker network can be used in the compose as
```yaml
networks:
  docker_net:
    external: true
```
`docker_net` is the name of docker network

then add network parameter as
```yaml
networks:
  - docker_net
```
