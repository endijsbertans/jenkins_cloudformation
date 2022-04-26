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
                
                    sh'''
                    git checkout main
                    git pull origin main
                     '''
                
            }
         }
         stage('send second stack to s3'){
               steps{
                  withAWS(region:'eu-west-1',credentials:'aws-creds') {
                  sh 'echo "Uploading content with AWS creds"'
                  s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'second-stack.yaml', bucket:'spainisdivi')

                    }
                    }
                    }

         stage('cloudformation update'){
                steps{
                    withAWS(region:'eu-west-1',credentials:'aws-creds') {                     
                     cfnUpdate(stack: 'tris', url: 'https://spainisdivi.s3.eu-west-1.amazonaws.com/second-stack.yaml')
                 }
                 }
                 }
     }
     post {

        success {
            echo 'I succeeded!'
        }


        aborted {
        withCredentials([gitUsernamePassword(credentialsId: 'git-creds', gitToolName: 'Default')]) {      
        echo "Hello, you aborted the mission ${env.USERNAME}!"
      
            
            sh '''
            
            git checkout main
            git pull origin main 
            git checkout -b revert
            git revert -m 1 HEAD
            git checkout main
            git merge revert
            
 
            git push origin main
            git branch -D revert
            '''
        }
        }
        } 
        
    
}


