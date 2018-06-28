pipeline {

    agent none

    stages {

        stage('hello') {
            agent any
            steps {
                sh 'echo Hello'
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
