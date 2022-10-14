pipeline {
  agent any

  parameters {
    choice(choices: ['Node 17', 'Node 18'], name: 'NODE_INSTALLATION_NAME')
  }

  environment {
    TOKEN = credentials("herokuToken")
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

    stage('deploy') {

      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'dockerId') {
            def image = docker.build('mohammaddocker/joke-app-jenkins')

            image.push('latest')
          }
        }
      }
    }
  }
}