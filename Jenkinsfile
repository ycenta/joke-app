pipeline {
    parameters {
      choice(name: 'NODE_VERSION', choices: ['16', '17'])
    }

    agent any

    environment {
      HEROKU_TOKEN = credentials('heroku_token')
      testPassed = true
      registry = "registry.heroku.com/joke-jenkins/web"
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
                  sh 'echo $testPassed'
                  sh 'npm install -g pnpm'
                  sh 'pnpm install'
                  sh 'pnpm build'
                  sh 'pnpm test'
                } catch(Exception e){
                    testPassed = false
                    stageResult = 'FAILURE'
                    buildResult = 'FAILURE'
                    throw e
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
          sh "export VERSION=\$(node -e \"console.log(require('./package.json').version)\""
          script {
            docker.build registry + ":$VERSION"
            docker.withRegistry( 'registry.heroku.com', 'herokuId' ) {
              dockerImage.push()
            }
          }
        }
      }
    }
}