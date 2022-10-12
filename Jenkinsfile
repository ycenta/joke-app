pipeline {
  agent any

  parameters {
      choice(name: 'NODE_VERSION', choices: ['18', '17'])
  }

  environment {
    tag = "registry.heroku.com/joke-jenkins/web"
  }

  stages {
    stage('build') {
      steps {
        nodejs(nodeJSInstallationName: "${params.NODE_VERSION}") {
          sh "node -v"
          sh "npm install"
          sh "npm run build"
          sh "npm test"
        }
      }
    }

    stage('deploy') {
      steps {
        script {
          docker.withRegistry('https://registry.heroku.com', 'herokuId') {
            sh "docker buildx build --platform linux/amd64 -t ${tag}:latest -t ${tag}:${BUILD_ID} ."

            sh "docker push ${tag}:latest"
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

