pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/SumitKumar121999/javatech']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t sksumit1999/dockertask-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerHub', variable: 'dockerHub')]) {
                   sh 'docker login -u sksumit1999 -p ${dockerHub}'

}
                   sh 'docker push sksumit1999/dockertask-integration'
                }
            }
        }
      
    }
}
