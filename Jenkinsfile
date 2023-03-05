pipeline {
     agent any
     triggers {
          pollSCM('* * * * *')
     }
     stages {
         
          stage("Docker build") {
               steps {
                    sh "docker run -v /var/run/docker.sock:/var/run/docker.sock"
                    sh "docker build -t leszko/calculator:latest2"
               }
          }
       }
    }
