// Define the URL of the Artifactory registry
def registry = 'https://trial28jeay.jfrog.io/'

pipeline {
  agent any
  
  environment {
    PATH="/opt/maven/bin:$PATH"  
  } 

  stages {
      
     stage("build") {
 
        steps {
          echo "---------- Build Start ----------"
          sh "mvn clean package -Dmaven.test.skip=true"
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
     // JFrog start
       stage("Jar Publish"){
          steps{
            script{
             echo '<--------------- Jar Publish Started --------------->'
                    def server = Artifactory.newServer url: registry + "/artifactory", credentialsId: "artifact-cred"
                    def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}"
                    def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "target/*.war",
                              "target": "sandyjfrog-libs-release-local",
                              "flat": "false",
                              "props": "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                    def buildInfo = server.upload(uploadSpec)
                    buildInfo.env.collect()
                    server.publishBuildInfo(buildInfo)
                    echo '<--------------- Jar Publish Ended --------------->'

        }

      }
  
    }  
    // Jfrog End
   


 
   }


 }
