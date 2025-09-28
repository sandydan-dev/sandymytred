pipeline {
  agent any
  
  environment {
    PATH="/opt/maven/bin:$PATH"  
  } 

  stages {
      
     stage("build") {
 
        steps {
          echo "---------- Build Start ----------"
          sh "mvn clean deploy -Dmaven.test.skip=true"
          echo "---------- Build End ----------"
        }
        
     }

     stage("test") {
          
         steps {
           echo "---------- Test Start ----------"
           sh "mvn surefire-report:report"   
           echo "---------- Test End ----------"
	}
	
      }

      stage("SonarQube Analysis"){
          
         environment {
            scannerHome = tool "sandy-sonar-scanner"
        }

         steps {
           withSonarQubeEnv("sandy-sonarqube-server"){
              sh "${scannerHome}/bin/sonar-scanner"
            }
         }
 
     }
 
   }


 }
