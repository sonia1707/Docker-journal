# Docker Journal
Apprentissage de Docker

# Mon Apprentissage Docker Complet

## Ce que j'apprends sur Docker

Ce repository contient toutes mes notes d'apprentissage sur Docker, des bases aux concepts avancés.

##  Mes notes par séance

### 1- Les bases
- Architecture Docker et concepts des containers
- Commandes de base Docker
- Premier container Nginx
- Gestion du cycle de vie des containers

### 2 - Concepts avancés  
- Gestion des images Docker
- Volumes et persistence des données
- Réseaux entre containers
- Containers interactifs

### 3 - Dockerfile
- Création d'images personnalisées
- Instructions Dockerfile (FROM, RUN, COPY, CMD)
- Build d'images
- Multi-stage builds

### 4 - Docker Compose
- Applications multi-conteneurs
- Fichier docker-compose.yml
- Gestion des services
- Réseaux et volumes dans Compose

### 5 - Production et Best Practices
- Docker Hub et registry
- Sécurité des containers
- Optimisation des images
- Monitoring et logging

---

#  Docker Cheatsheet - Référence Rapide
##  Commandes de Base
```bash
docker --version          # Version Docker
docker info               # Info système
docker system df          # Espace utilisé

. Gestion des IMAGES:
docker images            # Lister images locales
docker pull nginx:latest # Télécharger image
docker rmi nginx         # Supprimer image
docker image prune       # Nettoyer images inutilisées

.Gestion des CONTAINERS:
# Création
docker run -d --name web -p 8080:80 nginx
docker run -it ubuntu /bin/bash

# Gestion
docker start/stop/restart web
docker ps                 # Containers running
docker ps -a              # Tous containers

# Inspection
docker logs web          # Voir logs
docker stats web         # Statistiques
docker exec -it web bash # Shell interactif

# Suppression
docker rm web            # Supprimer
docker container prune   # Nettoyer arrêtés

.RÉSEAUX (Networks):
docker network ls              # Lister réseaux
docker network create mon-net  # Créer réseau
docker run --network mon-net nginx

.VOLUMES (Storage):
docker volume create mon-vol    # Créer volume
docker volume ls                # Lister volumes
docker run -v mon-vol:/data nginx

.DOCKERFILE:
FROM node:14-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]

.Build d'image:
docker build -t mon-app .          # Construire
docker build -t mon-app:1.0 .     # Avec tag

.DOCKER COMPOSE:
version: '3.8'
services:
  web:
    image: nginx
    ports: ["80:80"]
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: secret

.Commandes Compose:
docker-compose up -d        # Démarrer services
docker-compose down         # Arrêter services
docker-compose ps           # Statut services

.Exemples Pratiques:
1-Serveur web simple:
docker run -d --name webserver -p 80:80 nginx
# http://localhost:80
2-Base de données MySQL:
docker run -d --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=secret \
  -p 3306:3306 \
  mysql:8.0
3-Container interactif:
docker run -it --name my-ubuntu ubuntu:20.04 /bin/bash

.Dépannage Rapide:
1-Problèmes courants:
# Port déjà utilisé
docker run -p 8080:80 nginx    # Changer port
# Container ne démarre pas
docker logs container          # Voir erreurs
# Plus d'espace
docker system prune -a         # Nettoyage complet
2-Debugging:
# Mode interactif pour tester
docker run -it nginx /bin/bash
# Inspection complète
docker inspect container

.Commandes Avancées:
1-Optimisation:
# Limiter ressources
docker run --memory=512m --cpus=1.0 nginx
2-Sécurité:
# Exécuter en non-root
docker run --user 1000:1000 nginx
3-Docker Hub:
docker login                  # Connexion
docker tag app user/app       # Tagger
docker push user/app          # Publier

.Nettoyage:
# Nettoyage complet
docker system prune -a
# Nettoyage sélectif
docker container prune    # Containers arrêtés
docker image prune        # Images non utilisées
docker volume prune       # Volumes non utilisés

.Checklist Démarrage Rapide:
1-Vérifier installation : docker --version
2-Tester : docker run hello-world
3-Lancer un service : docker run -d -p 80:80 nginx
4-Vérifier : docker ps
5-Accéder : http://localhost:80

.Application web + base de données:
# Lancer MySQL
docker run -d --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=appdb \
  -p 3306:3306 \
  mysql:8.0

# Lancer l'application
docker run -d --name app \
  --link mysql-db:db \
  -p 8080:80 \
  mon-app

.Docker Compose multi-services:
version: '3.8'
services:
  frontend:
    build: ./frontend
    ports: ["3000:3000"]
  backend:
    build: ./backend  
    ports: ["5000:5000"]
  database:
    image: postgres:13
    environment:
      POSTGRES_PASSWORD: secret

.Best Practices:
1-Sécurités:
-Utiliser des images officielles
-Exécuter en non-root
-Mettre à jour régulièrement

2-Performances:
-Utiliser des images Alpine/slim
-Multi-stage builds
-Limiter les ressources

3-Maintenances:
-Tags versionnés pour les images
-Logs rotation
-Monitoring des ressources

.Dépannage Common:
1-Problèmes de ports:
# Vérifier les ports utilisés
netstat -tulpn | grep :80
# Changer le port
docker run -p 8080:80 nginx
2-Problèmes de volumes:
# Vérifier les volumes
docker volume ls
docker volume inspect mon-volume
3-Debugging:
# Logs détaillés
docker logs --tail 50 -f mon-container
# Inspection complète
docker inspect mon-container

.Resources utiles
-Documentation officielle : https://docs.docker.com/
-Docker Hub : https://hub.docker.com/
-Play with Docker : https://labs.play-with-docker.com/

