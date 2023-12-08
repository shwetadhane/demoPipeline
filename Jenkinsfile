pipeline {

  options {
    ansiColor('xterm')
  }
    tools{
        maven 'maven'
    }

    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -DskipTests=true clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

      stage('Deploy to Tomcat') {
          steps {
              script {
                def tomcatContainer = 'tomcat-manager'
                docker.image('maven:3.6.3-jdk-8').inside("-v ${env.WORKSPACE}:/usr/src/app") {
                    sh "docker cp ${env.WORKSPACE}/target/demo-pipeline.war ${tomcatContainer}:/usr/local/tomcat/webapps/"
                }
              }
           }
       }

    }
}