#!/bin/groovy
pipeline {
    agent any
    environment {
        repo_name = 'node-hello'
        tag = 'latest'
    }
    stages {
        stage('print env') {
            steps {
                sh 'env'
            }
        }
        stage('prepare') {
            steps {
                git url: "https://github.com/brownman/${repo_name}.git"
                sh 'ls -la'
            }
        }

        stage('Build Docker') {
            environment {
                DOCKERHUB_CREDENTIALS = credentials('docker_hub_access_token')
            }
            steps {
                        sh 'docker build -t $DOCKERHUB_CREDENTIALS_USR/$repo_name:$tag -f Dockerfile.echo .'
            }
        }

        stage('test') {
            agent {
                docker { image "${DOCKERHUB_CREDENTIALS_USR}/${repo_name}:${tag}" }
            }
            steps {
                docker.image('node:10-stretch').inside { c ->
                        echo 'Building..'
                        sh 'npm install'
                        echo 'Testing..'
                        sh 'npm test'
                        sh "docker logs ${c.id}"
                }
            }
        }

        stage('Push to docker hub (branch: master)') {
            when {
                // skip this stage unless branch is NOT master
                branch 'master'
            }
            environment {
                DOCKERHUB_CREDENTIALS = credentials('docker_hub_access_token')
            }

            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push $DOCKERHUB_CREDENTIALS_USR/$repo_name:$tag'
            //    sh 'docker logout'
            }
            post('logout') {
                always {
                        sh 'docker logout'
                }
            }
        } //stage
    } // stages
}
