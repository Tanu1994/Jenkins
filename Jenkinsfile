def didTimeout = false
def userInput = true
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
                    timeout(time: 15, unit: 'SECONDS') { // change to a convenient timeout for you
                        userInput = input(
                        id: 'Proceed1', message: 'Was this successful?', parameters: [
                        [$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']
                        ])
                    }
                } catch(err) { // timeout reached or input false
                    def user = err.getCauses()[0].getUser()
                    if('SYSTEM' == user.toString()) { // SYSTEM means timeout.
                        didTimeout = true
                    } else {
                        userInput = false
                        echo "Aborted by: [${user}]"
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
        always {
            script {
                if (didTimeout) {
                     currentBuild.result = 'SUCCESS'
                }
            }
        }
    }
}
