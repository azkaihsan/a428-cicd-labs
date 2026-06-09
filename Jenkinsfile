properties([
    pipelineTriggers([
        pollSCM('H/2 * * * *')
    ])
])

node {
    stage('Checkout') {
        checkout scm
    }

    stage('Install') {
        docker.image('node:18-alpine').inside {
            sh 'npm install'
        }
    }

    stage('Test') {
        docker.image('node:18-alpine').inside {
            sh 'CI=true npm test -- --watchAll=false'
        }
    }

    stage('Build') {
        docker.image('node:18-alpine').inside {
            sh 'npm run build'
        }
    }
}
