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
                        timeout(time: 20, unit: 'SECONDS') {
                            input 'Deploy to stage.'
                        }
                    }
                    catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException) {
                        didTimeout = true
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