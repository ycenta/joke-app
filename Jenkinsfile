pipeline {
  agent any

  stages {
    stage('build') {
      steps {
        sh "npm i -g pnpm"
        sh "pnpm install"
        sh "pnpm build"
        sh "pnpm test"
      }
    }
  }
}

