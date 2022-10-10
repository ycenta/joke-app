pipeline {
    parameters {
      choice(name: 'NODE_VERSION', choices: ['16', '17'])
    }

    agent any

    environment {
      HEROKU_TOKEN = credentials('heroku_token')
      testPassed = true
    }

    stages {
        stage('build-test') {
            when {
                expression {
                  BRANCH_NAME == 'main'
                }
            }

            steps {
              script {
                try{
                  sh 'npm install -g pnpm'
                  sh 'pnpm install'
                  sh 'pnpm build'
                  sh 'pnpm test'
                } catch(Exception e){
                    testPassed = false
                    stageResult = 'FAILURE'
                    buildResult = 'FAILURE'
                }
              }
            }
        }

      stage('docker') {
        when {
            expression {
              BRANCH_NAME == 'main'
              testPassed == true
            }
        }

        steps {
          sh "docker build . -t joke-app-jenkins:\$(node -e \"console.log(require('./package.json').version)\")"
        }
      }
    }
}