
pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          containers:
          - name: gradle
            image: gradle:6.3-jdk14
            command:
            - cat
            tty: true
          - name: docker
            image: docker:latest
            command:
            - cat
            tty: true
            volumeMounts:
             - mountPath: /var/run/docker.sock
               name: docker-sock
          volumes:
          - name: docker-sock
            hostPath:
              path: /var/run/docker.sock    
        '''
    }
  }
  stages {
    stage('Build-Gradle-Build') {
       steps {
         container('gradle') {
           try {
          sh 'chmod +x gradlew'
          sh './gradlew test'
          sh './gradlew jacocoTestCoverageVerification'
          sh './gradlew jacocoTestReport'
          sh './gradlew checkstyleMain'
          sh './gradlew build'
        }
            } catch (Exception E) {
             echo 'Failure detection'
           } 
      }  
            
   }
} 
}
