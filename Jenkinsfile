pipeline{
    agent any
    environment {
       dockerImageName = 'docker-package-only-build-demo'
       buildNumber = 'currentBuild.number'
       image_id = "nidhi221697/${dockerImageName}:${currentBuild.number}" 
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
       // stage('post build'){
       //    steps{
       //          sh 'docker build -t nidhi221697/${dockerImageName}:${buildNumber} .'
               //sh 'docker run -d -p 8084:8080 nidhi2/docker-package-only-build-demo:${currentBuild.number}'        
       //      }
       // } 
        stage('Build Docker Image'){  
            steps{
               sh "docker build -t ${dockerImageName} ."
      }
        }      
      stage('Tag'){ 
          steps{
              script{
          withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'dockerUSR', passwordVariable: 'dockerPWD')]) {
              sh "docker tag ${dockerImageName} ${dockerUSR}/${dockerImageName}:${currentBuild.number} " }
         }
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
      stage ('Deploy') {
          steps {
              script{
                  //image_id = "nidhi221697/${dockerImageName}:${currentBuild.number}"
                  sh "echo ${image_id}"
                  sh "ansible-playbook playbook.yml --extra-vars "image_id=${image_id}"
              }
          }
      }
   }
}
