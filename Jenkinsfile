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
    DB_HOST: credentials
    DB_USER: credentials
    DB_PASSWORD: credentials
    DB_PORT: credentials
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
