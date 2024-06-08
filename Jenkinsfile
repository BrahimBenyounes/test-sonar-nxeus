pipeline {
    agent any
    
    environment {
        // Define the path to Maven executable
        MAVEN_HOME = tool 'maven'
    }
    
    stages {
        stage("Build") {
            steps {
                // Clean and package the Maven project
                sh "${MAVEN_HOME}/bin/mvn clean package"
            }
        }
        
        stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    // Read Maven POM file
                    pom = readMavenPom file: "pom.xml"
                    
                    // Find artifacts to publish
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}")
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path
                    artifactExists = fileExists artifactPath
                    
                    if (artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}"
                        
                        // Publish artifacts to Nexus
                        nexusArtifactUploader(
                            nexusVersion: 'nexus3',
                            protocol: 'http',
                            nexusUrl: '192.168.186.129:8081',
                            groupId: 'pom.com.tn.esprit.rh',
                            version: pom.version,
                            repository: 'maven-central-repository',
                            credentialsId: 'nexus',
                            artifacts: [
                                [artifactId: pom.achat,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.achat,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        )
                    } else {
                        error "*** File: ${artifactPath}, could not be found"
                    }
                }
            }
        }
    }
}
