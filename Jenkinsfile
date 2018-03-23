pipeline {
  agent any
  stages {
    stage('initialization') {
      parallel {
        stage('initialization') {
          steps {
            echo 'Test'
          }
        }
        stage('Test') {
          steps {
            echo 'Hello'
          }
        }
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
        sh '''#!/bin/sh
docker push com.att.bc/api-gateway:latest'''
      }
    }
  }
  environment {
    version = '0.0.1'
  }
}