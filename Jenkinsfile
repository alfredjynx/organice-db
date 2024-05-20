pipeline {
    agent any
    environment {
        K8S_PORT = 53474
        TARGET = 'aws'
    }
    stages {
        stage('Jenkins db') {
            steps {
                echo 'Jenkins db'
            }
        }

        stage('Deploy on k8s') {
            when { 
                environment name: 'TARGET', value: 'local' 
            }
            steps {
                withCredentials([ string(credentialsId: 'minikube-credentials', variable: 'api_token') ]) {
                    sh "kubectl --token $api_token --server https://host.docker.internal:${env.K8S_PORT}  --insecure-skip-tls-verify=true apply -f ./k8s/configmap.yaml "
                    sh "kubectl --token $api_token --server https://host.docker.internal:${env.K8S_PORT}  --insecure-skip-tls-verify=true apply -f ./k8s/credentials.yaml "
                    sh "kubectl --token $api_token --server https://host.docker.internal:${env.K8S_PORT}  --insecure-skip-tls-verify=true apply -f ./k8s/pv.yaml "
                    sh "kubectl --token $api_token --server https://host.docker.internal:${env.K8S_PORT}  --insecure-skip-tls-verify=true apply -f ./k8s/pvc.yaml "
                    sh "kubectl --token $api_token --server https://host.docker.internal:${env.K8S_PORT}  --insecure-skip-tls-verify=true apply -f ./k8s/deployment.yaml "
                    sh "kubectl --token $api_token --server https://host.docker.internal:${env.K8S_PORT}  --insecure-skip-tls-verify=true apply -f ./k8s/service.yaml "
                }
            }
        } 


        stage('Deploy on AWS K8s') {
            when { 
                environment name: 'TARGET', value: 'aws' 
            }
            steps {
                withCredentials([ string(credentialsId: 'minikube-credential', variable: 'api_token') ]) {
                    sh "kubectl apply -f ./k8s/configmap.yaml"
                    sh "kubectl apply -f ./k8s/credentials.yaml"
                    sh "kubectl apply -f ./k8s/pv.yaml"
                    sh "kubectl apply -f ./k8s/pvc.yaml"
                    sh "kubectl apply -f ./k8s/deployment.yaml"
                    sh "kubectl apply -f ./k8s/service.yaml"
                }
            }
        }
    }
}