pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '''#!/bin/bash -e

git clone --depth 1 https://github.com/keycloak/keycloak.git'''
      }
    }
  }
}