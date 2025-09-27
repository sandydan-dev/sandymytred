pipeline {
  agent any
  
  environment {
    PATH="/opt/maven/bin:$PATH"  
  } 

  stages {
      
     stage("build") {
 
        steps {
          sh "mvn clean package"
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
