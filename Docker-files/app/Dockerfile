FROM openjdk:11 AS BUILD_IMAGE
RUN apt update && apt install -y maven git

# Clone the project and build it with Maven
RUN git clone https://github.com/devopshydclub/vprofile-project.git
WORKDIR /vprofile-project
RUN git checkout docker && mvn clean install

# Use the official Tomcat image for the second stage
FROM tomcat:9-jre11
LABEL "Project"="Vprofile"
LABEL "Author"="Laxman"

# Clean Tomcat's webapps directory
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy the .war file from the build stage to the Tomcat webapps directory
COPY --from=BUILD_IMAGE /vprofile-project/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

# Expose port 8080 and start Tomcat
EXPOSE 8080
CMD ["catalina.sh", "run"]
