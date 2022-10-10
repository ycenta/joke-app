pipeline {
    parameters {
      choice(name: 'NODE_VERSION', choices: ['16', '17'])
    }

    agent any

    environment {
      HEROKU_TOKEN = credentials('heroku_token')
      registry = "registry.heroku.com/joke-jenkins/web"
      VERSION = "latest"
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
              return BRANCH_NAME == 'main' 
            }
        }

        steps {
          sh "export VERSION=\$(node -e \"console.log(require('./package.json').version)\")"
          script {
            docker.withRegistry( 'https://registry.heroku.com', 'herokuId' ) {
              def image = docker.build("${env.registry}:${env.BUILD_ID}")
              image.push()
            }
          }
          sh "npm install -g heroku"
          withCredentials([usernamePassword(credentialsId: 'herokuId', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            sh "echo $USERNAME  echo $PASSWORD | heroku login"
            sh "heroku container:release web --app=joke-jenkins"
          }
        }
      }
    }
}