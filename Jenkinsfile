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
                sh 'mvn test site'
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
            }     
            
    }



