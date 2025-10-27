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
        stage('Docker images/conatiner remove') {
            steps {
                script{
                        sh '''docker stop javamavenapp_container
                        docker rm javamavenapp_container
                        docker rmi javamavenapp naveezzz/naveenmavenapp:latest'''
                }  
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
    '''post {
    always {
        script {
            def buildStatus = currentBuild.currentResult
            def buildUser = currentBuild.getBuildCauses('hudson.model.Cause$UserIdCause')[0]?.userId ?: 'Github User'
            
            emailext (
                subject: "Pipeline ${buildStatus}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    <p>This is a Jenkins maven CICD pipeline status.</p>
                    <p>Project: ${env.JOB_NAME}</p>
                    <p>Build Number: ${env.BUILD_NUMBER}</p>
                    <p>Build Status: ${buildStatus}</p>
                    <p>Started by: ${buildUser}</p>
                    <p>Build URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                to: 'mohdaseemakram19@gmail.com',
                from: 'mohdaseemakram19@gmail.com',
                replyTo: 'mohdaseemakram19@gmail.com',
                mimeType: 'text/html',
                attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
            )
           }
       }'''
    }
}
