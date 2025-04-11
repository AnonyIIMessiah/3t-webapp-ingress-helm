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
                        
                            
                            sh 'helm upgrade --install ingress ingress'
                        
                    }
            }   
        }
    }
}
