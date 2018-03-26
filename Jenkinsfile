pipeline {
  agent any
  stages {
    stage('Initialization') {
      steps {
        echo 'Hello'
      }
    }
    stage('Run Dockerfile.build ') {
      steps {
        sh 'docker build -t com.att.bc/api-gateway:build . -f Dockerfile.build'
      }
    }
    stage('Run Creator') {
      steps {
        sh 'docker container create --name extract com.att.bc/api-gateway:build'
      }
    }
    stage('Extract executable ') {
      steps {
        sh 'docker container cp extract:/go/src/api-gateway/main ./main'
      }
    }
    stage('Remove Creator') {
      steps {
        sh 'docker container rm -f extract'
      }
    }
    stage('Build final Image') {
      steps {
        sh 'docker build --no-cache -t com.att.bc/api-gateway:${params.BUILD_VERSION} .'
      }
    }
    stage('Clean leftovers') {
      steps {
        sh 'rm ./main'
      }
    }
    stage('Push Image') {
      steps {
        sh 'docker push com.att.bc/api-gateway:latest'
      }
    }
  }
  environment {
    version = '0.0.1'
  }
  parameters {
    string(name: 'PHASE', defaultValue: 'BUILD')
    string(name: 'APP_NAME', defaultValue: 'API-GATEWAY')
    string(name: 'BUILD_VERSION', defaultValue: '0.0.1')
    string(name: 'BRANCH_NAME', defaultValue: 'master')
  }
}