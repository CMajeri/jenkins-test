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
  - name: busy1
    image: busybox
    imagePullPolicy: Always
    command:
    - /bin/cat
    tty: true
  - name: busy2
    image: busybox
    imagePullPolicy: Always
    command:
    - /bin/cat
    tty: true

"""
    }
  }
  stages {
    stage('Test') {
      environment {
        HELLO = "World"
      }
      steps {
        git 'https://github.com/CMajeri/jenkins-test.git'
        container(name: 'busy1', shell: '/bin/sh') {
            sh '''
                env
                echo Hello $HELLO from 1 | nc 192.168.1.40 5000
                echo hello > test
            '''
        }
        container(name: 'busy2', shell: '/bin/sh') {
            sh '''
                env
                echo Hello $HELLO from 2 | nc 192.168.1.40 5000
                ls -la
                [ -f test ] && cat test
            '''
        }
      }
    }

    stage('Test2') {
      steps {
        container(name: 'busy2', shell: '/bin/sh') {
            sh '''
                env
                echo Hello again from 2 | nc 192.168.1.40 5000
                ls -la
                [ -f test ] && cat test
            '''
        }
      }
    }
  }
}
