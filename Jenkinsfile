node {
    properties([
        pipelineTriggers([
            pollSCM('H/2 * * * *')
        ])
    ])

    stage('Checkout') {
        checkout scm
    }

    stage('Install') {
        sh 'npm install'
    }

    stage('Test') {
        sh 'CI=true npm test -- --watchAll=false'
    }

    stage('Build') {
        sh 'npm run build'
    }
}
