pipeline {
    agent any
    stages {
        stage('Jenkins db') {
            steps {
                echo 'Jenkins db'
            }
        }

        stage('Deploy on k8s') {
            steps {
                withCredentials([ string(credentialsId: 'minikube-credentials', variable: 'api_token') ]) {
                    sh 'kubectl --token $api_token --server https://host.docker.internal:65096  --insecure-skip-tls-verify=true apply -f ./k8s/configmap.yaml '
                    sh 'kubectl --token $api_token --server https://host.docker.internal:65096  --insecure-skip-tls-verify=true apply -f ./k8s/credentials.yaml '
                    sh 'kubectl --token $api_token --server https://host.docker.internal:65096  --insecure-skip-tls-verify=true apply -f ./k8s/pv.yaml '
                    sh 'kubectl --token $api_token --server https://host.docker.internal:65096  --insecure-skip-tls-verify=true apply -f ./k8s/pvc.yaml '
                    sh 'kubectl --token $api_token --server https://host.docker.internal:65096  --insecure-skip-tls-verify=true apply -f ./k8s/deployment.yaml '
                    sh 'kubectl --token $api_token --server https://host.docker.internal:65096  --insecure-skip-tls-verify=true apply -f ./k8s/service.yaml '
                }
            }
        }   
    }
}