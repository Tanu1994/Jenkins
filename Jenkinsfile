pipeline {

    def didTimeout = false
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
                catch (err) {
                    def user = err.getCauses()[0].getUser()
                    if('SYSTEM' == user.toString()) { //timeout
                        didTimeout = true
                    }
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

    node {
        if (didTimeout) {
            currentBuild.result = 'SUCCESS'
            echo "Timeout"
        }
    }
}
