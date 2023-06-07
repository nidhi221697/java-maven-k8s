pipeline{
    agent any

    tools {
         maven 'myMavan'
         jdk 'java'
    }

    stages{
        stage('checkout'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github access', url: 'https://github.com/nidhi221697/java-maven-k8s.git']]])
            }
        }
        stage('build'){
            steps{
               sh 'mvn clean package'
            }
        }
        stage('post build'){
             steps{
               sh 'docker build -t nidhi/docker-package-only-build-demo:1.0.0 .'
               sh 'docker run -d -p 8084:8080 nidhi/docker-package-only-build-demo:1.0.0'        
             }
        } 
   }
}
