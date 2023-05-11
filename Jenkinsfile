pipeline {

  environment {
    dockerimagename = "emersonft/react-app"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'git remote add origin https://github.com/emersonft/teste_jenkins.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
        withDockerRegistry([ credentialsId: "dockerhub-credentials", url: "https://registry.hub.docker.com" ]) {
        dockerImage.push("latest")
        }
    } 

    // stage('Pushing Image') {
    //   environment {
    //            registryCredential = 'dockerhub-credentials'
    //        }
    //   steps{
    //     script {
    //       docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
    //         dockerImage.push("latest")
    //       }
    //     }
    //   }
    // }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

  }

}