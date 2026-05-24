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
                branch 'prod'
            }
            steps {
                sh 'docker build -t react-default:${BUILD_NUMBER} .'
            }
        }
        
        stage('Deploy') {
            when {
                branch 'prod'
            }
            steps {
                echo 'Deploy ke Minikube...'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline berhasil!'
        }
        failure {
            echo 'Pipeline gagal!'
        }
    }
}