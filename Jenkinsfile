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
               sh 'mvn package'
            }
        }
    }
}
