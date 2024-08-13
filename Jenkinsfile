pipeline {
    agent any 
    tools {
        maven 'mvn'
    }
         
    stages {
        stage('Build') { 
            steps {

                sh 'mvn clean build'
            }
        }
        stage('Test') { 
            steps {
                sh 'mvn test site'
            }
        }
            stage('package') {
                steps{
                sh 'mvn assemble'
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



