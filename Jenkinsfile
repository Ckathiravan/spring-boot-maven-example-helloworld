pipeline {
    agent any 
    stages {
        stage('Example Build') {
            agent { docker 'maven:3.8.1-adoptopenjdk-11' } 
            steps {
                echo 'Hello, Maven'
                sh 'mvn --version'
            }
        }
        stage('Example Test') {
            agent { docker 'openjdk:8-jre' } 
            steps {
                echo 'Hello, JDK'
                sh 'java -version'
            }
        }
        stage ('Build') {
               agent {
                      docker {
                      image 'maven:3.8.1-adoptopenjdk-11'
                      args '-v $HOME/.m2:/root/.m2'
                            }
                      }
               steps {
                    sh 'mvn -Dmaven.test.failure.ignore=true install' 
                     }
        }
        stage ('Build docker') {
            steps {
                echo 'Starting to build docker image'
			    script{
				sh 'docker build . -t ckathiravan/spring-boot:${env.BUILD_NUMBER}'
				withCredentials([string(credentialsId: 'docker-credential', variable: 'docker_password')]) 
                {
                sh '''docker login -u ckathiravan -p $docker_password
                docker push ckathiravan/spring-boot:${env.BUILD_NUMBER}'''
                }
            }
          }
       }
    
    }
}
