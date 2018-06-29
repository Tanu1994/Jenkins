pipeline {

    agent none
    stages {
        stage('hello') {
            agent any
            steps {
                sh 'echo Hello'
                lock('hello') {
                  echo 'Do something here that requires unique access to the resource'
                  // any other build will wait until the one locking the resource leaves this block
                }
            }
        }

        stage('approval') {
             agent none
             steps {
                milestone(ordinal: 1, label: "BOB")
                input 'Deploy to stage.'
             }
        }

        stage('hello again') {
             agent any
             steps {
                milestone(ordinal: 2, label: "BUILD_START_MILESTONE")
                sh 'echo Hello'
             }
        }

        stage('hello again again') {
            agent any
            steps {
                milestone(ordinal: 3, label: "BUILD_START_MILESTONE")
                sh 'echo Hello'
            }
        }
    }
}