pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }
    stages {
        

        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }
        
        stage('EKS-setup') {
            steps {
                script {
                    sh 'aws eks update-kubeconfig --name task-eks --region ap-south-1'
                }
            }
        }
        stage('Deploy') {
            
            steps {
                    script {
                        def releaseName = "ingress"
                        def namespace = "default"

                        def isInstalled = sh(
                            script: "helm list -n ${namespace} -q | grep -w ${releaseName} || true",
                            returnStdout: true
                        ).trim()

                        if (isInstalled) {
                            echo "Helm release '${releaseName}' is already installed."
                            sh 'helm upgrade ingress ingress'
                        } else {
                            echo "Helm release '${releaseName}' is not installed. Installing now..."
                            sh 'helm install ingress ingress'
                        }
                    }
            }   
        }
    }
}
