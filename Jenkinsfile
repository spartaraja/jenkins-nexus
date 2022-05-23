pipeline {
    agent any
    
    stages {
        stage("Clone code from GitHub") {
            steps {
                
                    git branch: 'main', credentialsId: 'git_test', url: 'https://github.com/spartaraja/jenkins-nexus.git';
                
            }
        }
        stage("Maven Build") {
            steps {
               
                    bat "mvn package"
                
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
                            nexusVersion: 'nexus3',
                            protocol: 'https',
                            nexusUrl: '1851-2401-4900-275f-461d-219d-ad5c-6cb4-fb06.in.ngrok.io',
                            groupId: 'com.mycompany.app',
                            version: '1.0-SNAPSHOT',
                            repository: 'newrepo',
                            credentialsId: 'admin',
                            artifacts: [
                                [artifactId: 'my-app',
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: 'my-app',
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } 
                }
            }
        }
    }
}
