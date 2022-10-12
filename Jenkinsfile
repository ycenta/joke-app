pipeline {
  agent any

  stages {
    stage('build') {
      steps {
        nodejs(nodeJSInstallationName: 'Node 17') {
              sh "npm install"
              sh "npm run build"
              sh "npm test"
            }
      }
    }
  }
}

