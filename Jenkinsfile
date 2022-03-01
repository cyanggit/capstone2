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
    stage('Provision AWS With Terraform') {
      steps {
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: "terraform", accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
          bat 'echo $AWS_ACCESS_KEY_ID'
          bat 'echo $AWS_SECRET_ACCESS_KEY'
          bat 'terraform init -input=false'
          bat 'terraform plan -out=tfplan -input=false'
          bat 'terraform apply -input=false tfplan'
        }
      }
    }
    stage('Deploy App to AWS') {
      steps {
          withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: "terraform", accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
          bat 'kubectl apply -f kubernetes.yml'
          bat 'kubectl get all'
        }
      }
    }
  }
}
