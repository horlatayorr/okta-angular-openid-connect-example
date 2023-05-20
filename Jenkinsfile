node {
    def nodeHome = tool name: 'node-18.16.0', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
    env.PATH = "${nodeHome}/bin:${env.PATH}"

    stage('check tools') {
        sh "node -v"
        sh "npm -v"
    }

    stage('checkout') {
        checkout scm
    }

    stage('npm install') {
        sh "npm install"
        sh "npm run build"
    }
    
    stage('npm install angular') {
        sh "npm install -g @angular/cli"
    }
  
    stage('unit tests') {
        sh "export CHROME_BIN=/usr/bin/google-chrome"// Set the CHROME_BIN environment variable
        sh "ng test --browsers=ChromeHeadlessNoSandbox" // Use Chrome Headless for testing
    }

    stage('protractor tests') {
        sh '''
                #!/bin/bash
                # Set up any necessary environment variables or configurations
                
                # Install test dependencies
                npm install --only=dev
                
                # Start the application (if needed)
                npm start &
                
                # Wait for the application to start (if needed)
                sleep 10
                
                # Run the end-to-end tests
                npm run e2e
                
                # Stop the application (if started)
                pkill -f "npm start"
            '''
    }

    stage('deploying') {
        sh '''
        # exit 1 on errors
        set -e
        # deal with remote
        echo "Checking if remote exists..."
        if ! git ls-remote heroku; then
          echo "Adding heroku remote..."
          git remote add heroku https://git.heroku.com/evening-meadow-46789.git
        fi
        # push only origin/master to heroku/master - will do nothing if
        # master doesn't change.
        echo "Updating heroku master branch..."
        git push heroku origin/master:master
        '''
    }
}
