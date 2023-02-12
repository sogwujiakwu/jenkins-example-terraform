pipeline {
  agent { label 'linux-ubuntu'}
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
    stage('terraform') {
      agent {
        docker { 
          image 'dokken/ubuntu-22.04:latest' 
          reuseNode true
          args '--entrypoint='
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
