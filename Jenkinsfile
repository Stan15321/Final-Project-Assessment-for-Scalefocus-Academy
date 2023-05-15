pipeline {
    agent any

    stages {
        stage('Check namespace') {
            steps {
                script {
                    def ns = sh(returnStdout: true, script: "kubectl get ns wp1 -o name || echo 'not found'")
                    if (ns.contains('not found')) {
                        sh "kubectl create ns wp1"
                    }
                }
            }
        }

        stage('Deploy WordPress') {
            steps {
                script {
                    def wp = sh(returnStatus: true, script: "helm list -n wp1 | grep wordpress || echo 'not found'")
                    if (wp.contains('not found')) {
                        sh "helm install final-project-wp-scalefocus wordpress/ -n wp1"
                    } else {
                        echo "WordPress already installed"
                    }
                }
            }
        }
    }
}