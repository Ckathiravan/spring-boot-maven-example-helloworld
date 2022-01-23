pipeline
{
    agent any 
    environment 
        {
            registry = "ckathiravan/spring-boot"
            registryCredential = 'dockerhub'
            dockerImage = ''
        }
    stages 
      {
                stage('Build') 
                  {
                    agent 
                      {
                         docker
                          {
                              image 'maven:3.8.1-adoptopenjdk-11'
                              args '-v $HOME/.m2:/root/.m2'
                          }
                      }
                    steps {
                            sh 'mvn -Dmaven.test.failure.ignore=true install' 
                          }
                    }
                stage('Building Docker Image') 
                  {
                    steps {
                            script { dockerImage = docker.build registry + ":$BUILD_NUMBER" }
                          }
                  }
                 stage('Push Image To Docker Hub') 
                  {
                     steps {
                         script {
                                  docker.withRegistry('', 'dockerhub') {
                                  dockerImage.push("${env.BUILD_NUMBER}")
                                  dockerImage.push("latest")
                             }
                         } 
                     }
                 }
      }
    post {
        always {
            sh 'docker logout'
        }
    }

}
