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

    stage('Manual Approval') {
        input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
    }

    stage('Deploy') {
        sh '''
            export NVM_DIR="$HOME/.nvm"
            . "$NVM_DIR/nvm.sh"
    
            export HOST=0.0.0.0
            export NODE_OPTIONS=--openssl-legacy-provider
    
            JENKINS_NODE_COOKIE=dontKillMe nohup npm start > react-app.log 2>&1 &
    
            sleep 20
            cat react-app.log || true
            netstat -tln 2>/dev/null | grep 3000 || true
        '''
    
        sleep(time: 1, unit: 'MINUTES')
    }
}
