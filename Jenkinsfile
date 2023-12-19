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
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
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

        stage('Initialize'){
             def dockerHome = tool 'myDocker'
             env.PATH = "${dockerHome}/bin:${env.PATH}"
        }
        stage('Docker Build') {
              steps {
      	      sh 'docker build -t madeeasyjava/service-registry:latest .'
              }
        }
       stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'dockerhub-creadentials', usernameVariable: 'madeeasyjava')]) {
        	    sh "docker login -u ${env.madeeasyjava} -p ${env.dockerhub-creadentials}"
                sh 'docker push madeeasyjava/service-registry:latest'
               }
            }
       }
   }
}





  
  
