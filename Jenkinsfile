pipeline {
     agent any
     triggers {
          pollSCM('* * * * *')
     }
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
          stage("Code coverage") {
               steps {
                    sh "./gradlew jacocoTestReport"
                    sh "./gradlew jacocoTestCoverageVerification"
               }
          }
          stage("Static code analysis") {
               steps {
                    sh "./gradlew checkstyleMain"
               }
          }
          stage("Package") {
               steps {
                    sh "./gradlew build"
               }
          }

          stage("Docker build") {
               steps {
                    sh "docker build -t leszko/calculator:${env.BUILD_TIMESTAMP} ."
               }
          }
           
          stage("install docker"){
               steps{
                    sh "docker run --name jenkins -p 8080:8080 -p 50000:50000
                        -v /var/run/docker.sock:/var/run/docker.sock
                        -v $(which docker):/usr/bin/docker
                        -v /home/jenkins_home:/var/jenkins_home
                          jenkins/jenkins"
               }
          }
          
          stage("Docker login") {
               steps {
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-hub-credentials',
                               usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                         sh "docker login --username mudassirmukhtar --password dckr_pat_hHGN8SHKbeECqJuyZML2ppOTKPU"
                    }
               }
          }

          stage("Docker push") {
               steps {
                    sh "docker push leszko/calculator:${env.BUILD_TIMESTAMP}"
               }
          }

          stage("Update version") {
               steps {
                    sh "sed  -i 's/{{VERSION}}/${env.BUILD_TIMESTAMP}/g' calculator.yaml"
               }
          }
       }
    }
