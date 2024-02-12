pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        sh 'docker build -t my-flask .'
        sh 'docker tag my-flask $DOCKER_BFLASK_IMAGE'
      }
    }
    stage('Test') {
      steps {
        sh 'docker run my-flask python -m pytest app/tests/'
      }
    }
    stage('Deploy') {
      steps {
        withCredentials([usernamePassword(credentialsId: "${DOCKER_REGISTRY_CREDS}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
          sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin docker.io"
          sh 'docker push $DOCKER_BFLASK_IMAGE'
        }
      }
    }
    
  }

 post {
        success {
            emailext (
                to: "shanmathivlr03@gmail.com",
                subject: "Build ${env.JOB_NAME} - ${env.BUILD_NUMBER} Successful",
                body: "The build succeeded. Congratulations!"
            )
        }
        unstable {
            emailext (
                to: "shanmathivlr@gmail.com",
                subject: "Build ${env.JOB_NAME} - ${env.BUILD_NUMBER} Unstable",
                body: "The build is unstable. Please check."
            )
        }
        failure {
            emailext (
                to: "shanmathivlr@gmail.com",
                subject: "Build ${env.JOB_NAME} - ${env.BUILD_NUMBER} Failed",
                body: "The build failed."
            )
        }
    }

}
  

