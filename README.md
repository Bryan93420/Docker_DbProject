# warrepo

Docker

Site de docker :
https://www.docker.com/docker-mac

Installation docker :
https://store.docker.com/editions/community/docker-ce-desktop-mac

Installer les images postgre et tomcat

——————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————


## Créer les Dockerfile permettant l’installation des applications et la copie des fichiers
vi Dockerfile
FROM tomcat:8-jre8

RUN apt-get update && apt-get install -y postgresql

COPY ./dbproject.war /usr/local/tomcat/webapps/

EXPOSE 8080


vi Dockerfile
FROM postgres:9.5

RUN apt-get update

COPY ./init-db.sql /docker-entrypoint-initdb.d/

EXPOSE 5432




## Monter les images modifiés
docker build -t postgres:9.5.4 .
docker build -t tomcat_1:1.1 .


## Démarrer les différentes images et lier le conteneur tomcat avec la base de données db
docker run -it --name db postgres:9.5.4
docker run -it --name tomcatdb -P --link db:db tomcat_1:1.1

## Mapper les ports
72d5617e3485        tomcat_3:1.0        "catalina.sh run"        34 minutes ago      Up 34 minutes       0.0.0.0:32775->8080/tcp   tomcat_3
ddd3b2afa607        postgre_2:1.0       "docker-entrypoint..."   36 minutes ago      Up 36 minutes       5432/tcp                  db

## Vérifier si la base de données ping sur tomcat
docker exec -it tomcatdb bash
ping db
exit

## Accéder à la page en tapant le port défini auparavant 
http://localhost:32775/dbproject/accueil.jsp
