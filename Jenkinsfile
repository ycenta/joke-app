pipeline {
    agent any

    stages {
        stage('build-test') {
            steps {
                sh 'pnpm install'
                sh 'pnpm build'
                sh 'pnpm test'
            }
        }
    }
}