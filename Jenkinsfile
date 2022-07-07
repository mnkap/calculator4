pipeline {
     agent any
     stages {
          stage("Compile") {
               steps {
                    sh "./gradlew compileJava"
               }
          }
          stage("Unit test") {
               steps {
                    sh "./gradlew test"
               }
          }
          stage("Package") {
               steps {
                    sh "./gradlew build"
               }
          }

          stage("Docker build") {
               steps {
                    sh "docker build -t leszko/calculator:${BUILD_TIMESTAMP} ."
               }
          }
          stage("Docker push") {
               steps {
                    sh "docker push leszko/calculator::${BUILD_TIMESTAMP}"
               }
          }
          
          stage("Deploy to staging") { 
               steps { 
                    sh "docker run -d --rm -p 8765:8080 --name calculator leszko/calculator" 
               } 
          }
          stage("Acceptance test") { 
               steps { 
                    sleep 60 
                    sh "./gradlew acceptanceTest -Dcalculator.url=http://localhost:8765"
               } 
          }
     }
     post { 
          always { 
               sh "docker stop calculator" 
          } 
     }
}
