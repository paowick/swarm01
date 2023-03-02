# swarm01

##### REF
This repo reference from ***https://github.com/docker/awesome-compose/tree/master/nginx-nodejs-redis***

Project structure:
```
.
├── README.md
├── docker-compose.yaml
├── nginx
│   ├── Dockerfile
│   └── nginx.conf
└── web
    ├── Dockerfile
    ├── package.json
    └── server.js
```
[_docker-compose.yaml_](docker-compose.yaml)
```

version: '3'
services:
  redis:
    image: 'redislabs/redismod'
    networks:
      - backend
  web1:
    image: 'paowick/swarm01_web:1.1'
    hostname: web1
    networks:
      - backend
  web2:
    image: 'paowick/swarm01_web:1.1'
    hostname: web2
    networks:
      - backend
  nginx:
    image: 'paowick/swarm01_nginx:1.1'
    depends_on:
    - web1
    - web2
    networks:
      - backend
      - webproxy
    deploy:
        replicas: 1
        labels:
          - traefik.docker.network=webproxy
          - traefik.enable=true
          - traefik.constraint-label=webproxy
          - traefik.http.routers.${APPNAME}-https.entrypoints=websecure
          - traefik.http.routers.${APPNAME}-https.rule=Host("${APPNAME}.xops.ipv9.me")
          - traefik.http.routers.${APPNAME}-https.tls.certresolver=default
          - traefik.http.services.${APPNAME}.loadbalancer.server.port=80
          - traefik.http.routers.${APPNAME}-https.tls=true
        resources:
          reservations:
            cpus: '0.1'
            memory: 10M
          limits:
            cpus: '0.4'
            memory: 250M

networks:
  webproxy:
    external: true
  backend:
    driver: overlay
```

#### Step

&emsp; **1**.clone <ins>*Nginx-Nodejs-Redis porject*</ins> from <ins>*awesome-compose*</ins>\
&emsp; **2**.build image form Dockerfile and push to **Docker Hub**
```
.
├── README.md
├── docker-compose.yaml
├── nginx
│   ├── Dockerfile  <--- here
│   └── nginx.conf
└── web
    ├──Dockerfile   <--- here!!
    ├── package.json
    └── server.js
```
&emsp; build image
```
$docker build -t paowick/swarm01_web:1.1 
$docker build -t paowick/swarm01_nginx:1.1
```
&emsp; push image to **Docker Hub**
```
$docker push paowick/swarm01_web:1.1
$docker push paowick/swarm01_nginx:1.1
```
&emsp; **3**.Deoly with docker-compose in https://portainer.ipv9.me/

## update on 2023 02 26
url https://spcn23swarm01.xops.ipv9.me/