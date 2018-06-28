def didTimeout = false
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
                    throw 0
                    timeout(time: 1, unit: 'MINUTES') {
                        input 'Deploy to stage.'
                    }
                }
                catch (err) {
                    def user = err.getCauses()[0].getUser()
                        didTimeout = true
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
        always {
            script {
                if (didTimeout) {
                     currentBuild.result = 'SUCCESS'
                }
            }
        }
    }
}
