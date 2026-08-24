pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/rashmigmr13-eng/node-todo-cicd.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t node-app .'
            }
        }

        stage('Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerHubCreds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker tag node-app $DOCKER_USER/node-app'
                    sh 'docker push $DOCKER_USER/node-app'
                }
            }
        }

        stage('Run') {
            steps {
                sh 'docker stop node-app || true'
                sh 'docker rm node-app || true'
                sh 'docker run -d -p 3000:8000 --name node-app node-app'
            }
        }
    }
}
