pipeline {
    agent any
    environement {
      HEROKU_TOKEN = credentials('heroku_token')
    }
    stages {

        stage('build-test') {
            when {
                expression {
                  BRANCH_NAME == 'main'
                }
            }
            steps {
                echo "${HEROKU_TOKEN}"
                sh 'pnpm install'
                sh 'pnpm build'
                sh 'pnpm test'
            }
        }
    }
}