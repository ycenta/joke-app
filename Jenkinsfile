pipeline {
    agent any
    
    // {
    //   docker { image 'node:17-alpine' }
    // }

    environment {
      HEROKU_TOKEN = credentials('heroku_token')
    }

    parameters {
      choice(name: 'NODE_VERSION', choices: ['16', '17'], defaultValue: '17')
    }
    stages {

        stage('build-test') {
            when {
                expression {
                  BRANCH_NAME == 'main'
                }
            }
            steps {
                sh "nvm use ${params.NODE_VERSION}"
                sh 'npm install -g pnpm'
                sh 'pnpm install'
                sh 'pnpm build'
                sh 'pnpm test'
            }
        }
    }
}