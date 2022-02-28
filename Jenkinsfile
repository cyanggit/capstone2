pipeline {
  agent any
  stages {
    stage('Docker Build') {
      steps {
         bat 'docker build -t yang97docker/cicd-project:latest .'
      }
    }
    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          bat "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          bat 'docker push yang97docker/cicd-project:latest'
         }
      }
    }
  }
}
