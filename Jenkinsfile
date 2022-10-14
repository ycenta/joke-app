pipeline {
  agent any

  stages {

    stage('build-test') {

      steps {
        nodejs(nodeJSInstallationName: 'Node 17') {
          sh 'node -v'
          sh 'pnpm install --frozen-lockfile'
          sh 'pnpm build'
          sh 'pnpm test'
        }
      }
    }
  }
}