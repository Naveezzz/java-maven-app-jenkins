pipeline {
    agent any
    
    
    tools{
        maven "Maven3"
    }

    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('git clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Naveezzz/java-maven-app-jenkins.git'
            }
        }
        stage('maven war file build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('docker image - push to dockerhub') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker', toolname: 'docker') {
                        
                        sh '''docker build -t javamavenapp .
                        docker tag javamavenapp naveezzz/naveenmavenapp:latest
                        docker push naveezzz/naveenmavenapp:latest'''
                     }
                 }
            }
        }
        stage('docker container of app') {
            steps {
               sh 'docker run -d -p 9000:8080 --name javamavenapp_container -t naveezzz/naveenmavenapp:latest'
            }
        }
    }
}
