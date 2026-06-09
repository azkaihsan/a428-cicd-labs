pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            args '--rm'
        }
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Install') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Test') {
            steps {
                sh 'CI=true npm test -- --watchAll=false'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
    }
    
    triggers {
        pollSCM('H/2 * * * *')
    }
}
