pipeline {
    agent any
    environment {
	dockerImage=""
	repository="130575395405.dkr.ecr.us-east-1.amazonaws.com"
	workerimage="${repository}/worker:${BUILD_NUMBER}"
	voteimage="${repository}/vote:${BUILD_NUMBER}"
	resultimage="${repository}/result:${BUILD_NUMBER}"
	}
    stages {
        stage("checkout"){
          steps{
              
             
			  checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/gupta5080/example-voting-app.git']]])
          }  
            
            
        }
    
        stage("Building Image"){
		   steps
		      { 
		          script 
		          
		               {
		                   dir('vote') 
						   {
                             // vote block
                        
                           dockerImage = docker.build voteimage
                        }
                        
                        dir('worker') 
						   {
                           // worker block
                         
                          
                           dockerImage = docker.build workerimage
                        }
                        
                        dir('result') 
						   {
                           // result block
                         
                          
                           dockerImage = docker.build resultimage
                        }
		               }
		        }
			}
			
			stage('Pushing to ECR') {
     steps{  
         script {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 130575395405.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker push "${resultimage}"'
                sh 'docker push "${voteimage}"'
                sh 'docker push "${workerimage}"'
      
         }
        }
      }
    }
}
