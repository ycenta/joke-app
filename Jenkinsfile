pipeline {
  agent any

  stages {
    stage('build-test') {
      steps {
        sh "npm install -g pnpm"
        sh "pnpm install"
        sh "pnpm build"
        sh "pnpm test"
      }
    }
  }
}
