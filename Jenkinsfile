pipeline {
    agent any
    
    tools {
        nodejs 'node20'
    }
    
    stages {
        stage('Clone') {
            steps {
                checkout scm
            }
        }
        
        stage('Install') {
            steps {
                sh 'node --version'
                sh 'npm install'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Docker Build') {
            when {
                branch 'production'
            }
            steps {
                sh 'docker build -t react-default:${BUILD_NUMBER} .'
                echo "Docker image react-default:${BUILD_NUMBER} berhasil dibuat!"
            }
        }
        
        stage('Deploy ke Minikube') {
            when {
                branch 'production'
            }
            steps {
                sh '''
                    kubectl apply -f k8s/deployment.yaml
                    kubectl rollout status deployment/react-default -n devops
                '''
            }
        }
    }
    
    post {
        success {
            echo "Pipeline branch ${BRANCH_NAME} berhasil!"
        }
        failure {
            echo "Pipeline branch ${BRANCH_NAME} gagal!"
        }
    }
}