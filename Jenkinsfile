pipeline {
  agent any

  parameters {
    choice(choices: ['Node 17', 'Node 18'], name: 'NODE_INSTALLATION_NAME')
  }

  stages {

    stage('build-test') {

      steps {
        nodejs(nodeJSInstallationName: "${params.NODE_INSTALLATION_NAME}") {
          sh 'node -v'
          sh 'npm install'
          sh 'npm run build'
          sh 'npm test'
        }
      }
    }
  }
}