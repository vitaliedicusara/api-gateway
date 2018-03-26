pipeline {
  agent any

  parameters {
    string(name: 'PHASE', defaultValue: 'BUILD')
    string(name: 'APP_NAME',defaultValue: 'API-GATEWAY')
    string(name: 'BUILD_VERSION',defaultValue: '0.0.1')
    string(name: 'BRANCH_NAME',defaultValue: 'master')
  }

  stages {
    stage('initialization') {
        steps {
            echo 'Hello'
        }
    }
    stage('Build') {
      steps {
        sh '''#!/bin/sh
echo Building com.att.bc/api-gateway:build
docker build -t com.att.bc/api-gateway:build . -f Dockerfile.build
docker container create --name extract com.att.bc/api-gateway:build
docker container cp extract:/go/src/api-gateway/main ./main
docker container rm -f extract
echo Building com.att.bc/api-gateway:latest
docker build --no-cache -t com.att.bc/api-gateway:latest .
rm ./main'''
      }
    }
    stage('Push Image') {
        steps {
        sh '''#!/bin/sh
docker push com.att.bc/api-gateway:latest'''
        }
    }
  }
  environment {
    version = '0.0.1'
  }
}
