pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: python
            image: python:3.8
        '''
    }
  }
  environment {
    DB_HOST = credentials('db-host')
    DB_USER = credentials('db-user')
    DB_PASSWORD = credentials('db-pass')
    DB_PORT = credentials('db-port')
  }
  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/shazforiot/nodeapp_test.git'
      }
    }

    stage('Build-Docker-Image') {
      steps {
        container('python') {
          sh 'pip install -r requirements.txt'
          sh 'sudo apt install gettext'
          sh 'envsubst < config.ini-template > config.ini'
          sh 'cat config.ini'
          sh 'python3 __init__.py'
        }
      }
    }
  }
}
