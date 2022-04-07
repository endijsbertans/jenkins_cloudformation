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
              
         stage('Update git repo') {
              steps {
                withCredentials([gitUsernamePassword(credentialsId: 'c91a80d0-4861-4985-b442-79e7779dd242', gitToolName: 'Default')]) {   
                    sh'''
                    git checkout main
                    git pull origin main
                     '''
                }
            }
         }
        stage('send second stack to s3'){
               steps{
                  withAWS(region:'eu-west-1',credentials:'b9a086b0-6dd2-4484-8e2d-25486f96a43a') {
                  sh 'echo "Uploading content with AWS creds"'
                  s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'second-stack.yaml', bucket:'spainisdivi')

                    }}}
                    stage('send second stack to s3'){
               steps{
                  withAWS(region:'eu-west-1',credentials:'b9a086b0-6dd2-4484-8e2d-25486f96a43a') {
                   sh'''
                    aws cloudformation update-stack --stack-name tris --template-url https://s3.amazonaws.com/spainisdivi/second-stack.yaml
                    '''
                    }}}
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


