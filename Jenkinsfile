pipeline {
  agent none

  triggers {
    pollSCM(' * * * * *')
  }

  stages {
    stage('Checkout') {
      agent any
      steps {
        git branch: 'main',
        url: 'https://github.com/lyciel-ydw/source-maven-java-spring-hello-webapp.git'
      }
    }
    stage('Build') {
      agent {
        docker { image 'maven:3-openjdk-17' }
      }
      steps {
        sh 'mvn clean package'
      }
    }
    stage('Image Build') {
      agent { label 'controller' }
      steps {
        sh 'docker image build -t tomcat:hello .'
      }
    }
    stage('Image Tag') {
      agent any
      steps {
        sh 'docker image tag tomcat:hello lyciel1229/tomcat:v1'
        sh 'docker image tag tomcat:hello lyciel1229/tomcat:latest'
      }
    }
    stage('Depoly') {
      agent { label 'controller' }
      steps {
        sh 'docker container run -d -p 80:8080 --name webserver tomcat:hello'
      }
    }
  }
}
