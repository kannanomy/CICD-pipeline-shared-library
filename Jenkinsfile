pipeline {
    agent any 
    tools {
        maven 'maven3'
    }
    environment {
        imageName = bg-sl // camel casing
        user_name = kannan65629 // snake casing
        ScannerName = tool 'sonarscanner'  // pascal casing 
    }
    stages {
        stage ('cleanup workspace') {
            steps {
                cleanWs() //clean workspace function
            }
        }
        stage ('compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage ('unit test') {
            steps {
                sh 'mvn test'
            }
        }
        stage ('static codeanalysis') {
            steps {
                withSonarQubeEnv ('sonarserver') {
                    sh''' $scanner/bin/sonar-scanner \
                    -Dsonar.projectName=bg-sl \
                    -Dsonar.projectKey=bg-slkey
                    -Dsonar.java.binaries=target/classes
                    '''
                }
            }
        }
        stage ('code package') {
            steps {
                sh 'mvn package'
            }
        }
        stage ('docker image build') {
            steps {
                sh 'docker build -t $user_name/$imageName .'
            }
        }
    }
}