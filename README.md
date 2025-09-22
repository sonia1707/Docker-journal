# Docker Journal
Apprentissage de Docker

#  Mon Apprentissage Docker Complet

##  Ce que j'apprends sur Docker

Ce repository contient toutes mes notes d'apprentissage sur Docker, des bases aux concepts avancés.

##  Mes notes par jour

### Jour 1 - Les bases
- Architecture Docker et concepts des containers
- Commandes de base Docker
- Premier container Nginx
- Gestion du cycle de vie des containers

### Jour 2 - Concepts avancés  
- Gestion des images Docker
- Volumes et persistence des données
- Réseaux entre containers
- Containers interactifs

### Jour 3 - Dockerfile
- Création d'images personnalisées
- Instructions Dockerfile (FROM, RUN, COPY, CMD)
- Build d'images
- Multi-stage builds

### Jour 4 - Docker Compose
- Applications multi-conteneurs
- Fichier docker-compose.yml
- Gestion des services
- Réseaux et volumes dans Compose

### Jour 5 - Production et Best Practices
- Docker Hub et registry
- Sécurité des containers
- Optimisation des images
- Monitoring et logging

##  Commandes importantes

### Vérification et information
```bash
docker --version                    # Version de Docker
docker info                        # Informations système
docker system df                   # Espace utilisé

.Gestion de images
docker images                      # Lister les images
docker pull nginx:latest           # Télécharger une image
docker rmi nginx                   # Supprimer une image
docker image prune                 # Nettoyer images inutilisées

.Gestion des containers
# Création et démarrage
docker run -d --name mon-nginx -p 8080:80 nginx
docker run -it ubuntu /bin/bash    # Mode interactif

# Gestion du cycle de vie
docker start mon-nginx             # Démarrer
docker stop mon-nginx              # Arrêter  
docker restart mon-nginx           # Redémarrer
docker pause mon-nginx             # Pauser

# Inspection et monitoring
docker ps                          # Containers en cours
docker ps -a                       # Tous les containers
docker logs mon-nginx              # Voir les logs
docker stats mon-nginx             # Statistiques ressources

# Commandes dans le container
docker exec -it mon-nginx bash     # Shell interactif
docker exec mon-nginx ls /app      # Commande unique

# Suppression
docker rm mon-nginx                # Supprimer container
docker container prune             # Nettoyer containers arrêtés

.Volumes Docker
docker volume create mon-volume    # Créer un volume
docker volume ls                   # Lister les volumes
docker run -v mon-volume:/data nginx  # Utiliser un volume
docker volume prune                # Nettoyer volumes

.Réseaux Docker
docker network ls                  # Lister les réseaux
docker network create mon-reseau   # Créer un réseau
docker run --network mon-reseau nginx  # Container sur réseau

.Dockerfile et build d'images
docker build -t mon-app .          # Construire une image
docker build -t mon-app:1.0 .     # Avec tag version

.Docker Compose
docker-compose up -d               # Démarrer les services
docker-compose down                # Arrêter les services  
docker-compose ps                  # Voir les services
docker-compose logs                # Voir les logs

.Docker Hub
docker login                       # Se connecter
docker tag mon-app user/mon-app    # Tagger une image
docker push user/mon-app           # Publier sur Docker Hub

.Exemples pratiques:
1-Application web simple:
# Lancer un serveur web
docker run -d --name web -p 80:80 nginx

# Accéder à l'application
# http://localhost:80

2-Application avec base de données:
# Base de données MySQL
docker run -d --name mysql-db -e MYSQL_ROOT_PASSWORD=secret -p 3306:3306 mysql:8.0

# Application connectée à la DB
docker run -d --name app --link mysql-db:db -p 8080:80 mon-app

.Fichiers de configuration:
1-Dockerfile exemple:
FROM node:14-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]

2-Docker Compose exemple:
version: '3.8'
services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
  backend:
    build: ./backend
    ports:
      - "5000:5000"
  database:
    image: postgres:13
    environment:
      POSTGRES_PASSWORD: secret


.Best Practices:
1-Sécurité
Utiliser des images officielles
Exécuter en non-root
Mettre à jour régulièrement

2-Performance
Utiliser des images Alpine/slim
Multi-stage builds
Limiter les ressources

3-Maintenance
Tags versionnés pour les images
Logs rotation
Monitoring des ressources

.Dépannage common:
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
Documentation officielle : https://docs.docker.com/
Docker Hub : https://hub.docker.com/


 ❗Important
Mon Docker a été supprimé suite à une réinitialisation de système. Je note toutes les commandes que j'apprends, mais je ne peux pas les tester tout de suite car je n'ai pas Docker sur mon ordinateur. Je les testerai quand je l'installerai.
Play with Docker : https://labs.play-with-docker.com/
