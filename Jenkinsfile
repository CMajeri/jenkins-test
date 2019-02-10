pipeline {
  agent {
    kubernetes {
      cloud 'kubernetes-int'
      label 'test'
      yaml """
kind: Pod
metadata:
  name: test
spec:
  hostNetwork: true
  containers:
  - name: busybox
    image: busybox
    imagePullPolicy: Always
    command:
    - /bin/cat
    tty: true
"""
    }
  }
  stages {
    stage('Busybox') {
      environment {
        HELLO = "World"
      }
      steps {
        git 'https://github.com/jenkinsci/docker-jnlp-slave.git'
        container(name: 'busybox', shell: '/bin/sh') {
            sh 'echo Hello $Hello | nc 192.168.1.40 5000'
        }
      }
    }
  }
}
