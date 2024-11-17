
pipeline {
    agent any
    stages {
        stage('Cleanup') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Git Repo') {
            steps {
                checkout scm
            }
        }

        stage('Listing files') {
            steps {
                sh 'ls -l'
            }
        }

        stage('Build and Push') {
            steps {
                echo 'Building..'
                dir('app'){
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-auth', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh '''
                            docker build -t 934372/todo-list-app:v1 .
                            docker login -u ${USERNAME} -p ${PASSWORD}
                            docker push 934372/todo-list-app:v1
                        '''
                    }
                }
            }
        }

        stage('Deploy container'){
            steps {
                echo "deploying container"
                sh 'docker stop todo-app || true && docker rm todo-app || true'
                sh 'docker run --name todo-app -d -p 3000:3000 934372/todo-list-app:v1'
            }
        }
    }
}
