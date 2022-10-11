pipeline {
  agent any
  parameters {
    choice(name: 'NODE_VERSION', choices: ['16', '17'])
  }

  environment {
    VERSION = "latest"
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
          try {
            sh "echo ${params.NODE_VERSION}"
            sh "npm install -g pnpm"
            sh "pnpm install"
            sh "pnpm build"
            sh "pnpm test"
          } catch (Exception e) {
            // mail(admin@gmail.com)
            throw e
          }
        }
      }
    }

    stage('deploy') {
      steps {
        script {
          VERSION = sh([script: "node -e \"console.log(require(\'./package.json\').version)\"", returnStdout: true]).trim()

          docker.withRegistry('https://registry.heroku.com', 'herokuId') {
            sh "docker buildx build --platform linux/amd64 -t ${registry}:$VERSION -t ${registry}:latest -t ${registry}:${BUILD_ID} ."
            sh "docker push --all-tags ${registry}"
          }
        }

        withCredentials([usernamePassword(credentialsId: 'herokuId', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            // sh "echo ${USERNAME}  echo ${PASSWORD} | heroku login"
            sh "HEROKU_API_KEY=${PASSWORD} npx heroku container:release web --app=joke-jenkins"
        }
      }
    }
  }
}
