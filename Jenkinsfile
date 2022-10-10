pipeline {
    parameters {
      choice(name: 'NODE_VERSION', choices: ['16', '17'])
    }

    agent any

    environment {
      HEROKU_TOKEN = credentials('heroku_token')
    }

    stages {
        def testPassed = true
        stage('build-test') {
            when {
                expression {
                  BRANCH_NAME == 'main'
                }
            }

            steps {
              try{
                sh 'npm install -g pnpm'
                sh 'pnpm install'
                sh 'pnpm build'
                sh 'pnpm test'
              } catch(Exception e){
                  testPassed = false
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