pipeline {
    
     agent any
     stages {

        stage('Input') {
                steps {
                     input('Do you want to proceed?')
                             }
                         }
            
              
         stage('Upload to AWS') {
              steps {
                  withAWS(region:'eu-west-1',credentials:'endijs') {
                  sh 'echo "Uploading content with AWS creds"'
                  s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'second-stack.yaml', bucket:'spainisdivi')
                  }
              }
         }
     }
     post {

        success {
            echo 'I succeeded!'
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            echo 'I failed :('
        }
        ABORTED {
            echo 'USER STOPPED ME :@'
        }
    }
}

