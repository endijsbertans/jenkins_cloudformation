pipeline {
    
     agent any
     stages {
             
        stage('Input') {
            steps{
              script {
                        env.USERNAME = input message: 'Please enter the username',
                                parameters: [string(defaultValue: '',
                                            description: '',
                                             name: 'Name:  ')]

        }
            
               
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
        aborted {
              
        echo "Hash of last merge: ${env.USERNAME}"
      
            
            echo "Sveiks ${env.USERNAME}, tu apstadinaji mani un man nacaas revertot!!!"
        }
        } 
        
    
}


