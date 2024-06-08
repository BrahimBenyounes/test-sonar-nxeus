pipeline {
    agent any

    environment {
        // Define the path to Maven executable
        MAVEN_HOME = tool 'maven'
    }

    stages {
        stage('Build') {
            steps {
                // Clean and package the Maven project
                sh "${MAVEN_HOME}/bin/mvn clean package"
            }
        }

        stage('Deploy to Nexus') {
            steps {
                // Deploy the artifact to Nexus repository
                sh "${MAVEN_HOME}/bin/mvn deploy"
            }
        }
    }
}
