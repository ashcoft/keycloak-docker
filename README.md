# keycloak-docker
Keycloak Docker Installation

version: "3.9"
services:
  postgres:
    image: postgres:latest
    restart: always
    volumes: 
      - ./pgdata:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: keycloak
      POSTGRES_USER: keycloak
      POSTGRES_PASSWORD: password
      

  keycloak:
    container_name: keycloak
    image: jboss/keycloak
    depends_on:
      - postgres
    ports:
      - 8080:8080
      - 8787:8787 # debug port
    networks:
      - backend
        
networks:
  backend:
    name: backend
    driver: bridge
    
    Administrator account

Okay, so Keycloak is running but we can’t do anything with it because we need to create an Administrator account. That we can also do with Docker.

While the compose setup is running, run this in your terminal:

docker exec keycloak \
    /opt/jboss/keycloak/bin/add-user-keycloak.sh \
    -u admin \
    -p admin \
&& docker restart keycloak
