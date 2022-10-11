pipeline {
  agent any

  stages {
    stage('build') {
      steps {
        sh "echo 'Building...'"
        script{
          sh "npm install -g"
          sh "pnpm install"
          sh "pnpm build"
          sh "pnpm test"
        }
      }

    }
  }
}