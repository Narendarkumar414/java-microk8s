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
                    sh "cd DOCKER; sudo docker build -t narendar414/devops-flow:v_${BUILD_NUMBER} ."
                    sh "sudo docker push narendar414/devops-flow:v_${BUILD_NUMBER}"
                }
            }
        }
        
        stage('image name update on microk8s') {
            steps {
                script {
                    sh "sudo microk8s  kubectl set image deployment.apps/java-api java-api=narendar414/devops-flow:v_${BUILD_NUMBER} --record"
                }
            }
        }
        
        stage('rollout status') {
            steps {
                script {
                    sh "sudo microk8s kubectl rollout status deployment/java-api"
                }
            }
        }
        
        stage('deleting all images') {
            steps {
                script {
                     try {
                    sh "sudo docker images > image.txt"
                    sh "sudo docker rmi `cat image.txt`"
                    } catch (Exception e) {
                         echo 'Exception occurred: ' + e.toString()
                         sh 'Handle the exception!'
                    }
                        
                }
            }
        }
       
    }
}
