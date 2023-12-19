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
            registry = "madeeasyjava/service-registry"
            registryCredential = 'dockerhub'
            dockerImage = ''
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
        stage('Build image') {
            steps{
            script {
             dockerImage = docker.build registry + ":$BUILD_NUMBER"
            }
           }
       }
        stage('Upload Image to Registry') {
            steps{
            script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
            }
          }
        }
    
     }
   }
}





  
  
