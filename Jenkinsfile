pipeline {
    agent any
    stages {
        def dockerImage

        stage('Lint') {
            steps {
                sh 'echo "Linting dockerfile"'
                sh 'hadolint-Linux-x86-64 Dockerfile'
                sh '/usr/local/bin/pylint --disable=R,C,W1203,E1120 app.py'                
            }
        }

        stage('Build image') {
            dockerImage = docker.build("jansdockerhub/streamlit-test:${env.BUILD_ID}")
        }

        stage('Push image') {
            dockerImage.push()
        }   

        stage('Deploy image') {
            steps {
                sh 'kubectl run streamlit-test --image=jansdockerhub/streamlit-test --port=8080'
            }
        }
        
    }
}