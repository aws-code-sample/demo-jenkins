// pipeline {
//   agent any
//   stages {
//     stage('build') {
//       steps {
//         echo 'Hello 世界'
//         sh '''#!/bin/bash
//           pwd
//           ls
//         '''
//       }
//     }
//   }
// }

podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      nodeSelector:
        karpenter-arch: arm64
      containers:
      - name: maven
        image: maven:3.8.1-jdk-8
        command:
        - sleep
        args:
        - 99d
      - name: golang
        image: golang:1.16.5
        command:
        - sleep
        args:
        - 99d
''') {
    node(POD_LABEL) {
      stage('Get a Maven project') {
        // git 'https://github.com/jenkinsci/kubernetes-plugin.git'
        container('maven') {
          stage('Build a Maven project') {
            sh 'mvn --version'
          }
        }
      }

      stage('Get a Golang project') {
        git url: 'https://github.com/aws-code-sample/demo-jenkins.git', branch: 'main'
        container('golang') {
          stage('Build a Go project') {
            sh '''
            go version
            ls
          '''
          }
        }
      }
    }
}

