pipeline {
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'java17'
        maven 'maven3'
    }
    environment{
            APP_NAME = "service-registry"
            RELEASE = "1.0.0"
            DOCKER_USER = "madeeasyjava"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD-NUMBER}"
           }
    
   stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }
        stage('Checkout from SCM'){
               steps{
               git branch:'master',credentialsId: 'github',url: 'https://github.com/MadeJavaEasy-Devops/service-registry'
               }
        }
        stage('Build Application'){
             steps{
             sh 'mvn clean package'
             }
        }
        stage('Test Application'){
             steps{
             sh 'mvn test'
             }  
        } 
        stage('Build and Push Docker Image'){
             steps{
                script{
                    docker.withRegistry('',DOCKER_PASS)
                    docker_image = docker.build "${IMAGE_NAME}"
                }
                   docker.withRegistry('',DOCKER_PASS){
                   docker_image.push("${IMAGE_TAG}")
                   docker-image.push('latest')                   
                   }
             }  
        } 
  }
}


  
  
