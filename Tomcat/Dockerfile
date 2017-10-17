FROM tomcat:8-jre8

RUN  apt-get update && apt-get install -y postgresql

COPY ./dbproject.war /usr/local/tomcat/webapps/

EXPOSE 8080
