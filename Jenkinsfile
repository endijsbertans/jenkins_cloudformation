pipeline {
    
     agent any
     stages {
         stage('Ask') {
             steps {
                timeout(time: 2, unit: “HOURS”) {
                input message: ‘Approve Deploy?’, ok: ‘Yes’
            } }
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
}
