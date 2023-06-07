pipeline{
    agent any
    environment {
       dockerImageName = 'docker-package-only-build-demo'
       buildNumber = 'currentBuild.number'
    }
     
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
                 sh 'docker build -t nidhi221697/${dockerImageName}:1.0.0 .'
               //sh 'docker run -d -p 8084:8080 nidhi2/docker-package-only-build-demo:1.0.0'        
             }
        } 
         stage('Publish') {
         // environment {
         //     registryCredential = 'dockerhub'
         // }
          steps{
              script {
                  withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'dockerUSR', passwordVariable: 'dockerPWD')]) {
                      sh "docker login -u ${dockerUSR} -p ${dockerPWD}"
                      sh "docker push ${dockerUSR}/${dockerImageName}:${currentBuild.number}" }

                 // def appimage = docker.build registry + ":$BUILD_NUMBER"
                 // docker.withRegistry( '', registryCredential ) {
                 //     appimage.push()
                 //     appimage.push('latest')
                //  }
              }
         }
      }
   }
}
