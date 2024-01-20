pipeline {
    agent any

    environment {
        GCP_PROJECT_ID = 'kubernetes-411705'
        GKE_CLUSTER_NAME = 'cluster-1'
        GKE_CLUSTER_ZONE = 'asia-south1-a'
        KUBE_CONFIG = "${WORKSPACE}/kubeconfig.yaml"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Add your build steps here
                docker build .
            }
        }

        stage('Deploy to GKE') {
            steps {
                script {
                    // Authenticate with GCP
                    withCredentials([gcpServiceAccount(credentialsId: 'kubernetes@kubernetes-411705.iam.gserviceaccount.com', project: GCP_PROJECT_ID)]) {
                        sh """
                            gcloud container clusters get-credentials ${GKE_CLUSTER_NAME} --zone ${GKE_CLUSTER_ZONE}
                        """
                    }

                    // Deploy to GKE using kubectl
                    sh """
                        kubectl apply -f path/to/kubernetes/deployment.yaml
                    """
                }
            }
        }
    }
}
