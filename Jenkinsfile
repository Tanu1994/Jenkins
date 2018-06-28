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
                 try {
                    timeout(time: 2, unit: 'MINUTES') {
                    input 'Deploy to stage.'
                    }
                 }
                 catch (err) {
                    def user = err.getCauses()[0].getUser()
                        if('SYSTEM' == user.toString()) { //timeout
                            currentBuild.result = "SUCCESS"
                    }
                 }
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
}
