pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        metadata:
          name: python
        spec:
          containers:
          - name: python
            image: python:3.8
            command: 
            - cat
            tty: true
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
        git 'https://github.com/EliasRemedi/test-jenkins.git'
      }
    }

    stage('run app') {
      steps {
        container('python') {
          sh 'pip install -r requirements.txt'
          sh 'apt update -y'
          sh 'apt install gettext -y'
          sh 'envsubst < config.ini-template > config.ini'
          sh 'cat config.ini'
          sh 'python3 __init__.py'
        }
      }
    }
  }
}
