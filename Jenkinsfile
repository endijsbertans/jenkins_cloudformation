pipeline {
    
     agent any
     stages {

        stage('Input') {
                steps {
                     input('Do you want to proceed?')
                             
                         
                             
                 script {
                        env.USERNAME = input message: 'Please enter the username',
                                parameters: [string(defaultValue: '',
                                            description: '',
                                             name: 'Hash of last merge: ')]

        }
        echo "Hash of last merge: ${env.USERNAME}"
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
        aborted {
            echo 'USER STOPPED ME :@'
            sh '''
            git rev-parse --short HEAD
            git revert -m 1

            '''
        }
    }
}

