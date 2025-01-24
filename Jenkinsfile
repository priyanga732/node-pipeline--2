pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    credentialsId: 'id_ed25519.pub', 
                    url: 'git@github.com:priyanga732/node-pipeline--2.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
    }
