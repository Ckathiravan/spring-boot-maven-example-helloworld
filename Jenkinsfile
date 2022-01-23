pipeline{

    agent {
               docker {
               image 'maven:3.8.1-adoptopenjdk-11'
               args '-v $HOME/.m2:/root/.m2'
          }
     }

    environment 
        {
            registry = "ckathiravan"
            registryCredential = 'docker-hub-credentials'
            dockerImage = ''
        }

	stages {
	    
	    stage('Build') {

			steps {
				sh 'mvn -Dmaven.test.failure.ignore=true install' 
			}
		}

		stage('Building Docker Image') {
			steps {
			   script {
                               dockerImage = docker.build registry + ":$BUILD_NUMBER"
                            }
			}
		   }

		stage('Push Image To Docker Hub') {

			steps {
				script {
          /* Finally, we'll push the image with two tags:
                   * First, the incremental build number from Jenkins
                   * Second, the 'latest' tag.
                   * Pushing multiple tags is cheap, as all the layers are reused. */
                         docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
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
