FROM openjdk:8 as BUILD_IMAGE
RUN apt autoremove
RUN apt update
RUN apt install  maven -y
RUN git clone https://github.com/automationlearner17/devops_kubernetes.git
RUN cd devops_kubernetes
RUN mvn install

WORKDIR /devops_kubernetes

FROM tomcat:8-jre11
RUN rm -rf /usr/local/tomcat/webapps/*
COPY --from=BUILD_IMAGE target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war
EXPOSE 8080
CMD ["catalina.sh","run"]
