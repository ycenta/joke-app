pipeline {
    agent any

    stages {

        stage('build-test') {
            when {
                expression {
                  BRANCH_NAME == 'main'
                }
            }
            steps {
                sh 'pnpm install'
                sh 'pnpm build'
                sh 'pnpm test'
            }
        }
    }
}