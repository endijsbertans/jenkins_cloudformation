pipeline {
    
     agent any
     stages {
             
        stage('Input') {
            steps{
              script {
                        env.USERNAME = input message: 'Please enter the username',
                                parameters: [string(defaultValue: '',
                                            description: '',
                                             name: 'Name: ')]

        }
            
               
                     input('Do you want to proceed?')
                             
    }
        }
              
         stage('Upload to AWS!') {
              steps {
                withCredentials([gitUsernamePassword(credentialsId: 'c91a80d0-4861-4985-b442-79e7779dd242', gitToolName: 'Default')]) {   
                    sh'''
                    git checkout main
                    git pull origin main
                     '''
                }
              }
              steps{
                  withAWS(region:'eu-west-1',credentials:'endijs') {
                  sh 'echo "Uploading content with AWS creds"'
                  s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'second-stack.yaml', bucket:'spainisdivi')
                   sh'''
                    aws cloudformation update-stack --stack-name tris --template-url https://s3.amazonaws.com/spainisdivi/second-stack.yaml
                    '''
                  }
                
              }
         }
     
     }
     
     post {

        success {
            echo 'I succeeded!'
        }


        aborted {
        withCredentials([gitUsernamePassword(credentialsId: 'c91a80d0-4861-4985-b442-79e7779dd242', gitToolName: 'Default')]) {      
        echo "Hello, you aborted the mission ${env.USERNAME}!"
      
            
            sh '''
            git branch -D revert
            git checkout main
            git pull origin main 
            git checkout -b revert
            git revert -m 1 HEAD
            git checkout main
            git merge revert
            
 
            git push origin main
            
            '''
        }
        }
        } 
        
    
}


