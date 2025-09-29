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
  
### 5 - Docker Swarm
- Orchestration de clusters
- Services réplicats
- Gestion des nœuds
- Déploiement en production

### 6 - Production et Best Practices
- Docker Hub et registry
- Sécurité des containers
- Optimisation des images
- Monitoring et logging
---

#  Docker Cheatsheet - Référence Rapide
##  Commandes de Base
```bash
docker --version          # Version du Docker
docker info               # Info sur le système
docker system df          # Espace utilisé ou occupé
```
### Gestion des IMAGES:
```bash
docker images            # Liste les images locales
docker pull nginx:latest # Télécharge d'image
docker rmi nginx         # Supprimer une image
docker image prune       # Nettoyer les images inutilisées
```
### Gestion des CONTAINERS:
```bash
# Création:
docker run -d --name web -p 8080:80 nginx
docker run -it ubuntu /bin/bash
# Gestion:
docker start/stop/restart web
docker ps                 # Containers running
docker ps -a              # Tous les containers
# Inspection:
docker logs web          # Voir les logs
docker stats web         # Statistiques
docker exec -it web bash # Shell interactif
# Suppression:
docker rm web            # Supprimer conteneur Docker
docker container prune   # Nettoye les conteneurs arrêtés
```
### RÉSEAUX (Networks):
```bash
docker network ls              # Liste les réseaux
docker network create mon-net  # Créer un réseau
docker run --network mon-net nginx
```
### VOLUMES (Storage):
```bash
docker volume create mon-vol    # Créer un volume
docker volume ls                # Liste les volumes
docker run -v mon-vol:/data nginx
```
### DOCKERFILE:
```bash
FROM node:14-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```
### Build d'image:
```bash
docker build -t mon-app .          # Construit une image Docker personnalisée
docker build -t mon-app:1.0 .     # Avec tag
```
### DOCKER COMPOSE:
```bash
version: '3.8'
services:
  web:
    image: nginx
    ports: ["80:80"]
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: secret
```
### Commandes Compose:
```bash
docker-compose up -d        # Démarre les services
docker-compose down         # Arrête les services
docker-compose ps           # Statut du service
```
### DOCKER SWARM - Orchestration:
1-Initialisation du Swarm:
```bash
# Initialiser le swarm (manager)
docker swarm init
# Rejoindre un swarm (worker)
docker swarm join --token <token> <manager-ip>:2377
# Afficher les noeuds
docker node ls
# Inspecter un noeud
docker node inspect <node-id>
```
2-Gestion des SERVICES:
```bash
# Créer un service
docker service create --name web -p 80:80 nginx
# Service avec réplicas
docker service create --name web --replicas 3 -p 80:80 nginx
# Lister les services
docker service ls
# Inspecter un service
docker service inspect web
# Voir les tâches d'un service
docker service ps web
# Mettre à jour un service
docker service update --replicas 5 web
# Scale un service
docker service scale web=10
# Supprimer un service
docker service rm web
```
3-STACKS (Docker Compose dans Swarm):
```bash
# docker-stack.yml
version: '3.8'
services:
  web:
    image: nginx
    ports:
      - "80:80"
    deploy:
      replicas: 3
      resources:
        limits:
          memory: 128M
        reservations:
          memory: 64M
      restart_policy:
        condition: on-failure
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: secret
    deploy:
      replicas: 1
      placement:
        constraints: [node.role == manager]
```
4-Commandes Stack:
```bash
# Déployer une stack
docker stack deploy -c docker-stack.yml myapp
# Lister les stacks
docker stack ls
# Lister les services d'une stack
docker stack services myapp
# Supprimer une stack
docker stack rm myapp
```
5-Gestion du CLUSTER:
```bash
# Afficher les nœuds
docker node ls
# Promouvoir un worker en manager
docker node promote <node-id>
# Rétrograder un manager en worker
docker node demote <node-id>
# Drainer un nœud (vider les containers)
docker node update --availability drain <node-id>
# Activer un nœud
docker node update --availability active <node-id>
```
6-RÉSEAUX dans Swarm:
```bash
# Créer un réseau overlay
docker network create -d overlay my-overlay
# Service sur réseau overlay
docker service create --name web --network my-overlay nginx
```
7-VOLUMES dans Swarm:
```bash
# Volume pour service (sur chaque nœud)
docker service create --name db \
  --mount type=volume,source=db-data,target=/var/lib/mysql \
  mysql:8.0
```
8-SÉCURITÉ Swarm:
```bash
# Rotation du token
docker swarm join-token --rotate worker
# Secrets management
echo "mon-secret" | docker secret create db_password -
# Service avec secret
docker service create --name app \
  --secret db_password \
  nginx
```
9-Exemples Swarm Complets:
```bash
-Déploiement d'application web:
# Créer le réseau overlay
docker network create -d overlay web-network
# Déployer le service web
docker service create --name webapp \
  --network web-network \
  --replicas 3 \
  -p 80:80 \
  nginx
# Déployer la base de données
docker service create --name database \
  --network web-network \
  --replicas 1 \
  -e MYSQL_ROOT_PASSWORD=secret \
  mysql:8.0
-Stack complète:
# docker-stack.yml
```bash
version: '3.8'
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    deploy:
      replicas: 3
      update_config:
        parallelism: 2
        delay: 10s
      rollback_config:
        parallelism: 1
        delay: 5s
      resources:
        limits:
          cpus: '0.50'
          memory: 512M
        reservations:
          cpus: '0.25'
          memory: 256M
      restart_policy:
        condition: on-failure
        delay: 5s
        max_attempts: 3
        window: 120s
    networks:
      - webnet

  visualizer:
    image: dockersamples/visualizer:latest
    ports:
      - "8080:8080"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]
    networks:
      - webnet

networks:
  webnet:
    driver: overlay
```
10.Commandes de MAINTENANCE:
```bash
# Mettre à jour un service
docker service update --image nginx:latest web
# Rollback d'un service
docker service rollback web
# Pause/resume de service
docker service update --update-pause web
docker service update --update-resume web
```
11.MONITORING Swarm:
```bash
# Voir les logs d'un service
docker service logs web
# Voir les logs en temps réel
docker service logs -f web
# Statistiques des services
docker stats $(docker service ps web -q)
```
12.Dépannage SWARM:
```bash
# Vérifier l'état du swarm
docker system info | grep -A 10 Swarm
# Voir les détails d'une tâche
docker service ps --no-trunc web
# Inspecter les conteneurs d'un service
docker ps --filter name=web
# Redémarrer les tâches défaillantes
docker service update --force web
```
### Exemples Pratiques:
1-Serveur web simple:
```bash
docker run -d --name webserver -p 80:80 nginx
# http://localhost:80
```
2-Base de données MySQL:
```bash
docker run -d --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=secret \
  -p 3306:3306 \
  mysql:8.0
```
3-Container interactif:
```bash
docker run -it --name my-ubuntu ubuntu:20.04 /bin/bash
```
### Dépannage Rapide:
1-Problèmes courants:
```bash
# Port déjà utilisé
docker run -p 8080:80 nginx    # Changer de port
# Container ne démarre pas
docker logs container          # Voir les erreurs
# Plus d'espace
docker system prune -a         # Nettoyage complet des ressources non utilisées dans Docker
```
2-Debugging:
```bash
# Mode interactif pour tester
docker run -it nginx /bin/bash
# Inspection complète
docker inspect container
```
### Commandes Avancées:
1-Optimisation:
```bash
# Limiter ressources
docker run --memory=512m --cpus=1.0 nginx
```
2-Sécurité:
```bash
# Exécuter en non-root
docker run --user 1000:1000 nginx
```
3-Docker Hub:
```bash
docker login                  # Connexion
docker tag app user/app       # Tagger
docker push user/app          # Publier
```
### Nettoyage:
```bash
# Nettoyage complet:
docker system prune -a
# Nettoyage sélectif:
docker container prune    # Containers arrêtés
docker image prune        # Images non utilisées
docker volume prune       # Volumes non utilisés
```
### Checklist Démarrage Rapide:
1-Vérifier installation:
```bash
docker --version
```
2-Tester:
```bash
docker run hello-world
```
3-Lancer un service:
```bash
docker run -d -p 80:80 nginx
```
4-Vérifier:
```bash
docker ps
```
5-Accéder:
```bash
http://localhost:80
```
### Application web + base de données:
1-# Lancer MySQL:
```bash
docker run -d --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=appdb \
  -p 3306:3306 \
  mysql:8.0
```
2-# Lancer l'application:
```bash
docker run -d --name app \
  --link mysql-db:db \
  -p 8080:80 \
  mon-app
```
### Docker Compose multi-services:
```bash
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
```
### Best Practices:
1-Sécurités:
```bash
-Utiliser des images officielles
-Exécuter en non-root
-Mettre à jour régulièrement
```
2-Performances:
```bash
-Utiliser des images Alpine/slim
-Multi-stage builds
-Limiter les ressources
```
3-Maintenances:
```bash
-Tags versionnés pour les images
-Logs rotation
-Monitoring des ressources
```
### Dépannage Common:
1-Problèmes de ports:
```bash
# Vérifier les ports utilisés
netstat -tulpn | grep :80
# Changer le port
docker run -p 8080:80 nginx
```
2-Problèmes de volumes:
```bash
# Vérifier les volumes
docker volume ls
docker volume inspect mon-volume
```
3-Debugging:
```bash
# Logs détaillés
docker logs --tail 50 -f mon-container
# Inspection complète
docker inspect mon-container
```
### Déploiement avec Docker
Prérequis:
```bash
Docker installé sur votre machine
Docker Compose pour les environnements multi-services(Optionnel)
```
Construction de l’image:
```bash
docker build -t nom-de-votre-app . # Crée une image Docker à partir du Dockerfile situé à la racine du projet.
```
Lancement du conteneur:
```bash
docker run -p 3000:3000 nom-de-votre-app # L’application sera accessible sur http://localhost:3000
```
Utilisation de Docker Compose:
```bash
docker-compose up --build # Lance tous les services si vous avez un fichier docker-compose.yml
```
Pour arrêter les conteneurs:
```bash
docker-compose down
```
Variables d’environnement:
```bash
# Crée un fichier .env à la racine du projet pour définir les variables qui seront automatiquement chargées par Docker Compose
PORT=3000
DB_PASSWORD=secret
```
Nettoyage:
```bash
docker system prune -a # Supprime tout ce qui est inutile et à utiliser avec précaution
```
### Exemple de Déploiement Node.js+MongoDB
📁Structure du projet:
```bash
docker-journal/
├── app/
│   ├── server.js
│   └── package.json
├── Dockerfile
├── docker-compose.yml
└── .env
```
🐳Dockerfile:
```bash
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```
⚙️docker-compose.yml:
```bash
version: '3.8'
services:
  web:
    build: ./app
    ports:
      - "3000:3000"
    env_file:
      - .env
    depends_on:
      - mongo
    networks:
      - app-net
  mongo:
    image: mongo:6.0
    volumes:
      - mongo-data:/data/db
    networks:
      - app-net
volumes:
  mongo-data:
networks:
  app-net:

```
🔐.env:
```bash
PORT=3000
MONGO_URL=mongodb://mongo:27017/mydb
```
🚀Déploiement:
```bash
# Build et lancement
docker-compose up --build -d
# Vérifier les services
docker-compose ps
# Accéder à l'app
http://localhost:3000
```
🧹Nettoyage:
```bash
docker-compose down -v # Arrêt complet et suppression des volumes
```
### Resources utiles
-Documentation officielle : https://docs.docker.com/
-Docker Hub : https://hub.docker.com/
-Play with Docker : https://labs.play-with-docker.com/

