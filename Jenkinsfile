pipeline {
    agent any

    stages {

        stage ('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("thiagosalvador/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }
        stage ('push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dokerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
            }
            }
        
        }

        stage ('Deploy kubernetes') {
            steps {
                withkubeConfig ([credentialsId: 'kubeconfig']){
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                }
            }
        }

    }

}
