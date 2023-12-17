pipeline {
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'java17'
        maven 'maven3'
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
  }
}


  
  
