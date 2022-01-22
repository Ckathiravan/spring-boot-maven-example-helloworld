pipeline 
  {
   agent 
     {
               docker {
               image 'maven:3.8.1-adoptopenjdk-11'
               args '-v $HOME/.m2:/root/.m2'
               }
     }
   stages {
        stage('mvn version') {
            steps {
                echo 'Hello, Maven'
                sh 'mvn --version'
            }
          }
        stage('Java Version') {
            agent { docker 'openjdk:8-jre' } 
            steps {
                echo 'Hello, JDK'
                sh 'java -version'
                  }
        }
        stage ('Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true install' 
                  } 
          }
        stage ('Build docker') {
            steps {
                echo 'Starting to build docker image'
			    script{
				sh 'docker build . -t ckathiravan/spring-boot:test'
				withCredentials([string(credentialsId: 'credential ', variable: 'docker_password')]) 
                {
                sh '''docker login -u ckathiravan -p $docker_password
                docker push ckathiravan/spring-boot:test'''
                }
            }
          }
       }
    }
}
