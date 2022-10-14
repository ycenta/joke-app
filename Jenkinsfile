pipeline {
  agent any

  parameters {
    choice(choices: ['Node 17', 'Node 18'], name: 'NODE_INSTALLATION_NAME')
  }

  environment {
    TOKEN = credentials("herokuToken")
    tag = "registry.heroku.com/joke-jenkins/web"
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
          def image = docker.build('mohammaddocker/joke-app-jenkins')

          docker.withRegistry('https://registry.hub.docker.com', 'dockerId') {
            image.push('latest')
          }

          docker.withRegistry('https://registry.heroku.com', 'herokuId') {
            image.tag("${tag}:latest")
            image.push()
          }
        }
      }
    }
  }
}