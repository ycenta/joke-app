pipeline {
  agent any

  stages {

    stage('build-test') {

      steps {
        nodejs(nodeJSInstallationName: 'Node 17') {
          sh 'node -v'
          sh 'npm install'
          sh 'npm run build'
          sh 'npm test'
        }
      }
    }
  }
}