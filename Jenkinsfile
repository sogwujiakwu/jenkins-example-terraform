pipeline {
  agent { label 'linux-centos'}
  options {
    skipDefaultCheckout(true)
  }
  stages{
    stage('clean workspace') {
      steps {
        cleanWs()
      }
    }
    stage('checkout') {
      steps {
        checkout scm
      }
    }
    stage('tfsec') {
      agent {
        docker { 
          image 'tfsec/tfsec-ci:v0.57.1' 
          reuseNode true
        }
      }
      steps {
        sh '''
          tfsec . --no-color
        '''
      }
    }
    stage('terraform') {
      agent {
        docker { 
          image 'dokken/ubuntu-22.04:latest' 
          reuseNode true
        }
      }      
      steps {
        sh './terraformw apply -auto-approve -no-color'
      }
    }
  }
  post {
    always {
      cleanWs()
    }
  }
}
