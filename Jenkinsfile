pipeline {

    agent none

    stages {

        stage('hello') {
            agent any
            steps {
                sh 'echo Hello'
            }

        }

        stage('approval') {
             agent none
             steps {
                script {
                try {
                    timeout(time: 1, unit: 'MINUTES') {
                    input 'Deploy to stage.'
                    }
                }
                finally {
                    currentBuild.result = 'SUCCESS'
                    sh 'echo I am here'
                }}
             }
         }

        stage('hello again') {
            agent any
            steps {
                milestone(ordinal: 1, label: "BUILD_START_MILESTONE")
                sh 'echo Hello'
            }

        }

    }

    post {
        aborted {
          script {
            currentBuild.result = 'SUCCESS'
          }
        }
    }
}
