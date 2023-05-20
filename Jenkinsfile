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
    }
    
    stage('npm install angular') {
        sh "npm install -g @angular/cli"
    }
  
    stage('unit tests') {
        sh "export CHROME_BIN=/usr/bin/google-chrome"// Set the CHROME_BIN environment variable
        sh "ng test --browsers=ChromeHeadlessNoSandbox" // Use Chrome Headless for testing
    }
    
    stage('Set Chrome Environment Variable') {
        // Set the CHROME_BIN environment variable to the Chrome browser binary location
        sh "export CHROME_BIN=/usr/bin/google-chrome"
    }

    stage('Build') {
        // Build the Angular project
        sh 'ng build --prod'
      }
  
    stage('Run') {
        // Serve the Angular project using a web server
        sh 'ng serve --prod'
    }

}
