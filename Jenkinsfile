pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '''#!/bin/bash -e

git clone https://github.com/keycloak/keycloak.git'''
      }
    }
  }
}