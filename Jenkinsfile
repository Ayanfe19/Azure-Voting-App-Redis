pipeline {
    agent any
    environment {
        registryName = 'azure-vote-front'
        registryCredential = 'acr-cred'
        registryUrl = 'ayanfeacr.azurecr.io'
        dockerImage = 'ayanfeacr.azurecr.io/azure-vote-front:v3'
    }

    stages {
        stage('Build Image') {
            steps {
                script {
                    dockerImage = sh('docker build -t ayanfeacr.azurecr.io/azure-vote-front:v3 ./azure-vote')
                }
            }
        }

        stage('Upload Image to ACR') {
            steps {
                script {
                    docker.withRegistry("http://${registryUrl}", registryCredential) {
                        sh('docker push ayanfeacr.azurecr.io/azure-vote-front:v3')
                    }
                }
            }
        }

        stage('K8S Deploy') {
            steps {
                script {
                    withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8S-cicd', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                        sh('kubectl apply -f azure-vote-all-in-one-redis.yaml')
                    }
                }
            }
        }
    }
}
