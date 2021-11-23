#!/bin/groovy
 


pipeline {
    agent any
    stages {
        stage("test") {
            steps {
                script {
                    test_try_catch()
                 }
            }
        }
    }
}
def test_function() { println("hello World")}
def test_branch(){
        if (env.BRANCH_NAME == 'master') {
            echo 'I only execute on the master branch'
        } else {
            echo 'I execute elsewhere'
        }
}
def test_try_catch(){
        try {
            sh 'exit 1'
        }
        catch (exc) {
            echo 'Something failed, I should sound the klaxons!'
            throw
        }
}
 
