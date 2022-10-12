pipeline {
  agent any

  stages {
    stage('build') {
      steps {
        nodejs(nodeJSInstallationName: 'Node 17.x') {
              sh "npm install"
              sh "npm run build"
              sh "npm test"
            }
      }
    }
  }
}

