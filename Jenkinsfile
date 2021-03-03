pipeline {
    agent any
    stages {
        stage('list repo files') {
            steps {
                script {
                    sh "ls"
                }
            }
        }
        stage('show working directory') {
            steps {
                script {
                    sh "pwd"
                }
            }
        }
        stage('creating build') {
            steps {
                script {
                    sh "./gradlew clean build"
                }
            }
        }
        stage('list build contents') {
            steps {
                script {
                    sh "ls build/libs"
                }
            }
        }
        stage ('remove old jar from Docker') {
            steps {
                script {
                    sh "rm DOCKER/spring-boot-with-prometheus-0.1.0.jar"
                }
            }
        }
        
        stage('copy buid file to docker build context') {
            steps {
                script {
                    sh "cp /var/lib/jenkins/workspace/mk10s/build/libs/spring-boot-with-prometheus-0.1.0.jar DOCKER/"
                }
            }
        }
        stage('build docker image and pushing to dockerhub') {
            steps {
                script {
                    sh "cd DOCKER; sudo docker build -t harshvardhan1402/devops-flow:v_${BUILD_NUMBER} ."
                    sh "sudo docker push harshvardhan1402/devops-flow:v_${BUILD_NUMBER}"
                }
            }
        }
       
    }
}
