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
                    //customImage.push('latest')

                    withDockerRegistry([ credentialsId: "jenkinscred", url: "" ]) {
                        customImage.push()
                    }
                }
            }
        }
        
        stage('Deploy image') {
            steps {
                //sh "/usr/bin//kubectl cluster-info"
                //sh "echo $KUBECONFIG"
                //sh "kubectl version"
                //sh "kubectl get nodes"
                //sh "kubectl run nginx  --replicas=2 --labels='app=nginx' --image=nginx --port=80"
                sh "/usr/bin/kubectl --kubeconfig=/kubecfg/test.conf run streaml --labels='app=streamlit-test' --image=jansdockerhub/streamlit-test:${env.BUILD_ID} --port=8501"
                sh '/usr/bin/kubectl --kubeconfig=/kubecfg/test.conf create -f loadbalancer.yaml'
                //kubectl get service/strlit-service |  awk {'print $1" " $2 " " $4 " " $5'} | column -t
            }
        }
        
    }
}