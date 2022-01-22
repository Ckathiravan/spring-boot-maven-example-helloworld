pipeline{

      agent {
                docker {
                image 'maven:openjdk'

                }
            }
        
        stages{

              stage('Quality Gate Status Check'){
                  steps{
                      script{
		    	    sh "mvn clean install"
		  
                 	    }
               	        }  
              }	
		
            }	       	     	         
}
