pipeline {
  agent {
    kubernetes {
      yamlFile "jenkins-pod.yaml"
    }
  }
  environment {
    PROJECT = "jenkins-demo"
    REGISTRY_USER = "noushadck"
  }
  stages {
    stage("Build") {
      steps {
        container("kaniko") {
          sh "/kaniko/executor --context `pwd` --destination noushadck/jenkins-demo:latest --destination ${REGISTRY_USER}/${PROJECT}:${env.BRANCH_NAME.toLowerCase()}-${BUILD_NUMBER}"
            sh "echo Build Done "
        }
      }
    }
    stage("Test") {
      when { changeRequest target: "master" }
      steps {
        container("kustomize") {
          sh """
            set +e
            echo "Hello its Working"
          """
        }
      }
    }
    stage("Deploy") {
      when { branch "master" }
      steps {
        container("kustomize") {
          sh """
            echo "We are inside Kustomize"
          """
        }
      }
    }
  }
}
