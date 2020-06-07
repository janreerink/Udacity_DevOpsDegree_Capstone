pipeline {
    agent any
    stages {
        
        stage('Lint') {
            steps {
                sh 'echo "Linting dockerfile"'
                sh 'hadolint-Linux-x86-64 Dockerfile'
                sh '/usr/local/bin/pylint --disable=R,C,W1203,E1120 app.py'                
            }
        }

        stage('Build image') {
            steps {
                script {
                    def customImage = docker.build("jansdockerhub/streamlit-test:${env.BUILD_ID}")
                    withDockerRegistry([ credentialsId: "jenkinscred", url: "" ]) {
                        customImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy image') {
            steps {
                //initial deployment:
                //sh '/usr/bin/kubectl --kubeconfig=/kubecfg/test.conf create deployment mlapp --image=jansdockerhub/streamlit-test:${env.BUILD_ID}'
                //sh '/usr/bin/kubectl --kubeconfig=/kubecfg/test.conf  expose deployment/mlapp --type=LoadBalancer --port=8501'
                
                //rolling update:
                sh '/usr/bin/kubectl --kubeconfig=/kubecfg/test.conf set image deployments/mlapp mlapp=jansdockerhub/streamlit-test:latest'
                //kubectl get service/strlit-service |  awk {'print $1" " $2 " " $4 " " $5'} | column -t
            }
        }
        
    }
}