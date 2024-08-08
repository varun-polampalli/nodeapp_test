docker run -u 0 --privileged --name jenkins -it -d -p 8080:8080 -p 50000:50000 \
 -v /var/run/docker.sock:/var/run/docker.sock \
 -v $(which docker):/usr/bin/docker \
 -v /home/jenkins_home:/var/jenkins_home \
 jenkins/jenkins:latest


pipeline {

  environment {
    dockerimagename = "varunpolampalli2001/nodeapp"
    dockerImage = ""
  }

  agent any

  stages {

      stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}
