properties([
    pipelineTriggers([
        pollSCM('H/2 * * * *')
    ])
])

node {
    stage('Setup Node.js') {
        sh '''
            curl -fsSL https://deb.nodesource.com/node_18.x/nodesource.gpgkey | apt-key add -
            echo "deb https://deb.nodesource.com/node_18.x nodestream main" | tee /etc/apt/sources.list.d/nodesource.list
            apt update
            apt install -y nodejs
        '''
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
