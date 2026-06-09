properties([
    pipelineTriggers([
        pollSCM('H/2 * * * *')
    ])
])

node {
    stage('Checkout') {
        checkout scm
    }

    stage('Setup Node.js') {
        sh '''
            export NVM_DIR="$HOME/.nvm"
            if [ ! -d "$NVM_DIR" ]; then
                curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | sh
            fi
            . "$NVM_DIR/nvm.sh"
            nvm install 18
            nvm use 18
        '''
    }

    stage('Install') {
        sh '''
            export NVM_DIR="$HOME/.nvm"
            . "$NVM_DIR/nvm.sh"
            npm install
        '''
    }

    stage('Build') {
        sh '''
            export NVM_DIR="$HOME/.nvm"
            . "$NVM_DIR/nvm.sh"
            export NODE_OPTIONS=--openssl-legacy-provider
            npm run build
        '''
    }

    stage('Test') {
        sh '''
            export NVM_DIR="$HOME/.nvm"
            . "$NVM_DIR/nvm.sh"
            CI=true npm test -- --watchAll=false
        '''
    }
    
}
