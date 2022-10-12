pipeline {
  agent any

  environment {
    VERSION = "latest"
    registry = "registry.heroku.com/joke-jenkins/web"

    HEROKU_API_KEY = credentials('HEROKU_API_KEY')
  }

  stages {
    stage('build') {

      steps {
        sh "npm install -g pnpm"
        sh "pnpm install"
        sh "pnpm build"
        sh "pnpm test"
      }

    }
    stage ('deploy') {
      steps {
        script { 
          docker.withRegistry('https://registry.heroku.com', 'herokuId') {
            sh "docker buildx build --platform linux/amd64 -t ${registry}:latest -t ${registry}:${BUILD_ID} ."
            sh "docker push --all-tags ${registry}"
          }
        }

        sh "HEROKU_API_KEY=${HEROKU_API_KEY} npx heroku container:release web --app=joke-jenkins"
      }
		}

  }
}
