pipeline {
  agent any

  parameters {
      choice(name: 'NODE_VERSION', choices: ['18', '17'])
  }

  environment {
    tag = "registry.heroku.com/joke-jenkins/web"
  }

  stages {
    stage('build') {
      steps {
        nodejs(nodeJSInstallationName: "${params.NODE_VERSION}") {
          sh "node -v"
          sh "npm install"
          sh "npm run build"
          sh "npm test"
        }
      }
    }

    stage('deploy') {
      steps {
        script {
          docker.withRegistry('registry.heroku.com', 'herokuId') {
            def image = docker.build("${env.tag}")
            image.push()
          }
        }
      }
    }
  }
}

