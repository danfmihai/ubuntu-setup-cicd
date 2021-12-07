# Setup a Java CICD pipeline with Jenkins

- This application is java spring boot web application

- Sets a Jenkinsfile pipeline for the Gradle Java app.

- Uses SonarQube pluging for quality checks (static code analysis)

- Builds a Docker image that's pushed to the Nexus registry

- To build the Java app we use ```./gradlew build ``` that will generate a war file. This war file will be placed in a tomcat webserver image to make it run.