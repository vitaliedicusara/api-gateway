pipeline {
  agent any
  stages {
    stage('Run Dockerfile.build') {
      parallel {
        stage('Run Dockerfile.build') {
          steps {
            echo 'Building docker image'
            sh 'docker build -t com.att.bc/api-gateway:build . -f Dockerfile.build'
            echo 'Start Creator container --name ${params.BUILD_ID}'
            sh 'docker container create --name ${params.BUILD_ID} com.att.bc/api-gateway:build'
            echo 'Extract creator container --name ${params.BUILD_ID}'
            sh 'docker container cp ${params.BUILD_ID}:/go/src/api-gateway/main ./main'
            echo 'Remove the creator container --name ${params.BUILD_ID}'
            sh 'docker container rm -f ${params.BUILD_ID}'
            echo 'build the com.att.bc/api-gateway:${params.BUILD_ID}'
            sh 'docker build --no-cache -t com.att.bc/api-gateway:${params.BUILD_ID} .'
            echo 'push the com.att.bc/api-gateway:${params.BUILD_ID}'
            sh 'docker push com.att.bc/api-gateway:latest'
          }
        }
        stage('error') {
          agent {
            dockerfile {
              filename 'Dockerfile.build'
            }
            
          }
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
      }
    }
  }
  parameters {
    string(name: 'PHASE', defaultValue: 'BUILD')
    string(name: 'APP_NAME', defaultValue: 'API-GATEWAY')
    string(name: 'VERSION', defaultValue: '0.0.1')
    string(name: 'BRANCH_NAME', defaultValue: 'master')
  }
}