pipeline {
  agent any

  environment {
    VERSION = "latest"
  }

  stages {
    stage('build') {
      steps {
        sh "echo 'Building... version : ${env.VERSION}'"
        sh "echo 'Building... version : ${VERSION}'"
        sh "npm install -g pnpm"
        sh "pnpm install"
        sh "pnpm build"
        sh "pnpm test"
      }

    }
  }
}
