# Traefik Docker Swarm

### Deployment

```
docker swarm init --advertise-addr 1.2.3.4
docker stack deploy -c docker-compose.yml --with-registry-auth traefik
```

### Removal

```
docker stack rm traefik
docker swarm leave -f
```

### Adding a service

```yaml
version: "3.7"
services:
  container:
    image: container-image:1.0
    deploy:
      mode: replicated
      replicas: 1
      labels:
        - traefik.enable=true
        - traefik.http.routers.container-router.entrypoints=websecure
        - traefik.http.routers.container-router.rule=Host(`example.com`)
        - traefik.http.services.container-service.loadbalancer.server.port=80
      restart_policy:
        condition: on-failure
        delay: 5s
        max_attempts: 3
        window: 10s
    networks:
      - traefik-network

networks:
  traefik-network:
    external: true
```
