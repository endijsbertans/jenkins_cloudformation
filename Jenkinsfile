pipeline {
    
     agent any
     stages {
             
        stage('Input') {
            steps{
              script {
                        env.USERNAME = input message: 'Please enter the username',
                                parameters: [string(defaultValue: '',
                                            description: '',
                                             name: 'Hash of last merge: ')]

        }
            
               
                     input('Do you want to proceed?')
                             
    }
        }
              
         stage('Upload to AWS!') {
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

        failure {
            echo 'I failed :('
        }
        aborted {
        withCredentials([gitUsernamePassword(credentialsId: '123', gitToolName: 'Default')]) {      
        echo "Hello, you aborted the mission ${env.USERNAME}!"
      
            
            sh '''
            git branch -D revert
            git checkout main
            git pull origin main 
            git checkout -b revert
            git revert -m 1 HEAD
            git checkout main
            git merge main
            
 
            git push origin main
            
            '''
        }
        }
        } 
        
    
}


