pipeline {
  agent any
  environment {
    // Set the KUBECONFIG environment variable
    KUBECONFIG = 'C:/Users/stani/.kube/config'
  }
  stages {
    stage('Verify') {
      steps {
        // This command verifies that the authentication and configuration are working correctly
        bat 'kubectl get services'
      }
    }
    stage('Deploy : final-project-wp-scalefocus.') {
      steps {
        script {
          try {
            // Check if the namespace exists
            def namespaceExists = bat(script: 'kubectl get namespace default', returnStatus: true) == 0
            if (!namespaceExists) {
              // Create the namespace
              bat 'kubectl create namespace wp'
            }
            else
            {
                bat 'kubectl get services'
            }
            // Deploy the application using Helm
            bat 'helm dependency build ./bitnami/wordpress'
            bat 'helm install final-project-wp-scalefocus ./bitnami/wordpress -f ./bitnami/wordpress/values.yaml'
          } catch (Exception e) {
            // Handle any deployment errors
            error "Deployment failed: ${e.getMessage()}"
          }
        }
      }
    }
  }
}
