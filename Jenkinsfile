#!/bin/groovy


pipeline {
  agent any
  environment {
      repo_name = 'node-hello'
      tag = 'latest'
      DOCKERHUB_CREDENTIALS = credentials('docker_hub_access_token')
  }
  stages {
    stage('intro') {
      steps {
        echo 'test pipeline message'
      }
    }
    stage('prepare') {
      steps{
      git url:"https://github.com/brownman/node-hello.git"
      sh "ls -la;env"
      

      }
    }
    
    stage('Build Docker'){
        steps {
                        sh 'docker build -t $DOCKERHUB_CREDENTIALS_USR/$repo_name:$tag -f Dockerfile.echo .'
        
        }
    }
            stage('test') {
            agent {
              docker { image "${DOCKERHUB_CREDENTIALS_USR}/${repo_name}:${tag}" }
            }
            steps {
                sh 'npm run test'
            }
        }
    
    
        stage('Login'){
        steps {
                        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'

        }
    }
    
           stage('push image to docker hub'){
 when {
        // skip this stage unless branch is NOT master
          branch "master"

      }
      
        steps {
                        sh 'docker push $DOCKERHUB_CREDENTIALS_USR/$repo_name:$tag'

        }
    }
    

    
  }
  
                 post('logout'){
        always {
                        sh 'docker logout'

        }
    }
    
    
}
