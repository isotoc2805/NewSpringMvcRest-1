pipeline {
    agent any
        stages {
        stage('Initialize'){
            steps{
                echo "Esta es el inicio"
            }
        }
            
             stage('Test2') {
            steps {
                 sh 'cat Jenkinsfile'
            
            }
        } 
            
        stage('Build') {
            steps {
                sh 'mvn -B package'
            
            }
        }
            
        stage('Test') {
            steps {
                 sh "mvn clean verify" 
            
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
                            nexusVersion: "nexus3",
                            protocol: "http",
                            nexusUrl: "192.168.1.86:8081",
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: "maven-releases",
                            credentialsId: "NexusCredentials",
                            artifacts: [
                                [artifactId: pom.artifactId,
                                        classifier: '',
                                        file: artifactPath,
                                        type: pom.packaging]
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
