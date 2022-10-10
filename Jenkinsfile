pipeline {
    parameters {
      choice(name: 'NODE_VERSION', choices: ['16', '17'])
    }

    agent {
      docker { image 'node:17-alpine' }
    }

    environment {
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
                sh 'npm install -g pnpm'
                sh 'pnpm install'
                sh 'pnpm build'
                sh 'pnpm test'
            }
        }
    }
}