pipeline {
    
    agent any
    
    stages {
        
        stage ('Build Docker Image') {
            steps {
                script {
                    docker_app = docker.build("rafaelmartinisilva/kube-news:${env.BUILD_NUMBER}", '-f ./src/Dockerfile ./src')
                }
            }
        }

        stage ('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub') {
                        docker_app.push("${env.BUILD_NUMBER}")
                        docker_app.push("latest")
                    }
                }
            }
        }

        stage ('Deploy Kubernetes') {
            steps {
                withKubeConfig ([credentialsId: 'kube_config']) {
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                }                
            }
        }
    }
}