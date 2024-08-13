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
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
               
        }
        stage('Test') { 
            steps {
                sh 'mvn test site'
            }
        }
            stage('package') {
                steps{
                sh 'mvn assemble:single'
            }
            }
                stage('Sonarqube Analysis') {
            
            environment {
                scannerHome = tool 'scanner'
            }
            steps {
                withSonarQubeEnv('sonarserver') {
                    sh """${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey= \
                    -Dsonar.projectName= \
                    -Dsonar.projectVersion=1.0 \
                    -Dsonar.sources=employee/src \
                    
                    -Dsonar.sourceEncoding=UTF-8 \
                    """
                }
            }
        }
            }     
            
    }



