pipeline {
    agent any 
    tools {
        maven 'maven3'
    }
         
    stages {
        stage('Build') { 
            steps {

               sh 'mvn clean install -DskipTests'
            }
                 post {
                success {
                    echo 'Archiving'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
               
        }
        stage('Test') { 
            steps {
                sh 'mvn test'
            }
        }
            stage('package') {
                steps{
                sh 'mvn clean package'
            }
            }
                stage('Sonarqube Analysis') {
            
            environment {
                scannerHome = tool 'scanner'
            }
            steps {
                withSonarQubeEnv('sonarserver') {
                    sh """${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey=javaappl\
                    -Dsonar.projectName=javaappl\
                    -Dsonar.projectVersion=1.0 \
                    -Dsonar.sources=src \
                    
                      """
                }
            }
        }
        stage('Upload Artifact') {
            
             steps {
                script {
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',                
                        protocol: 'http',
                        nexusUrl: '3.87.43.198:8081',
                        
                        
                        version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                        repository: 'javaappl',
                        credentialsId: 'nexuslogin',
                        artifacts: [
                          [artifactId: 'javaappl',
                            classifier: '',
                        
                            file: 'target/practise1.war',
                            type: 'war']
                        ]
                    )
                }
            }     
            
    }
    }
}


