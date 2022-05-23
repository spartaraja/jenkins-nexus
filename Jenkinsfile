pipeline {
    agent any
    
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "https"
        NEXUS_URL = "1851-2401-4900-275f-461d-219d-ad5c-6cb4-fb06.in.ngrok.io"
        NEXUS_REPOSITORY = "newrepo"
        NEXUS_CREDENTIAL_ID = "admin"
    }
    stages {
        stage("Clone code from GitHub") {
            steps {
                script {
                    git branch: 'main', credentialsId: 'git_test', url: 'https://github.com/spartaraja/jenkins-nexus';
                }
            }
        }
        stage("Maven Build") {
            steps {
                script {
                    sh "mvn package -DskipTests=true"
                }
            }
        }
        stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: https,
                            nexusUrl: 1851-2401-4900-275f-461d-219d-ad5c-6cb4-fb06.in.ngrok.io,
                            groupId: pom.com.mycompany.app,
                            version: pom.1.0-SNAPSHOT,
                            repository: newrepo,
                            credentialsId: admin,
                            artifacts: [
                                [artifactId: pom.my-app,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.my-app,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }
    }
}
