# First stage: Build the application
FROM openjdk:11 AS BUILD_IMAGE

# Install Maven
RUN apt update && apt install -y maven git

# Clone the repository
RUN git clone https://github.com/SirDominique/vprofile-project1.git

# Change directory, checkout the docker branch, and build the project
WORKDIR /vprofile-project1
RUN git checkout docker && mvn clean install

# Second stage: Prepare runtime image
FROM tomcat:9-jre11
LABEL "Project"="Vprofile"
LABEL "Author"="SirDominique"

# Remove default webapps
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy the WAR file from the build stage
COPY --from=BUILD_IMAGE /vprofile-project1/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

# Expose the port
EXPOSE 8080

# Start Tomcat
CMD ["catalina.sh", "run"]
