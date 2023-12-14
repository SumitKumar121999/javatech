pipeline {
    agent any
    tools {
        maven 'maven_3_5_0'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/SumitKumar121999/javatech']]])
                sh 'mvn clean install'
            }
        }

        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t sksumit1999/dockertask-integration .'
                }
            }
        }

        stage('Push image to Hub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerHub', variable: 'dockerHub')]) {
                        sh 'docker login -u sksumit1999 -p ${dockerHub}'
                    }
                    sh 'docker push sksumit1999/dockertask-integration'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonartoken2', variable: 'SONAR_TOKEN')]) {
                    sh "mvn sonar:sonar -Dsonar.projectKey=tasklast -Dsonar.host.url=http://172.23.148.54:9590 -Dsonar.login=${env.SONAR_TOKEN}"
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
