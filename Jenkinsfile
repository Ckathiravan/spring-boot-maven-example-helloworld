pipeline{
       agent any
        
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
