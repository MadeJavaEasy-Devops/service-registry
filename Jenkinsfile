pipeline {
    agent any

    tools {
        maven "maven3"
    }

    stages {
        stage('Checkout & Maven Build') {
            steps {
                // Get some code from a GitHub repository
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/MadeJavaEasy-Devops/service-registry.git']])

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

            }
        }
     
        stage('Build Docker Image') {
            steps{
              script{
                  sh 'docker build -t madeeasyjava/service-registry .'
              }
            }
        }
        stage('Push Docker Image') {
            steps{
              script{
                  withCredentials([string(credentialsId: 'docker-id', variable: 'dockerhubPWD')]){
                  sh 'docker login -u madeeasyjava -p ${dockerhubPWD}'
              } 
                  sh 'docker push madeeasyjava/service-registry'
             }
           }
       }
    }
}
