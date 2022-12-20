FROM openjdk:8 as BUILD_IMAGE
RUN apt autoremove
RUN apt update
RUN apt install  maven -y
RUN apt install git -y
RUN git clone -b master https://github.com/automationlearner17/devops_kubernetes.git
RUN cd devops_kubernetes && mvn install 


FROM tomcat:8-jre11
RUN rm -rf /usr/local/tomcat/webapps/*
COPY --from=BUILD_IMAGE devops_kubernetes/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war
EXPOSE 8080
CMD ["catalina.sh","run"]
