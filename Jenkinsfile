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
            # Install Node.js 18 tanpa sudo (dengan nvm)
            curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
            export NVM_DIR="$([ -t 1 ] && echo -nE $'^[]B' && echo -nE "$HOME/.nvm" || echo "$HOME/.nvm")"
            source "$NVM_DIR/nvm.sh"
            nvm install 18
            nvm use 18
            nvm alias default 18
        '''
    }

    stage('Install') {
        sh '''
            export NVM_DIR="$HOME/.nvm"
            source "$NVM_DIR/nvm.sh"
            npm install
        '''
    }

    stage('Test') {
        sh '''
            export NVM_DIR="$HOME/.nvm"
            source "$NVM_DIR/nvm.sh"
            CI=true npm test -- --watchAll=false
        '''
    }

    stage('Build') {
        sh '''
            export NVM_DIR="$HOME/.nvm"
            source "$NVM_DIR/nvm.sh"
            npm run build
        '''
    }
}
