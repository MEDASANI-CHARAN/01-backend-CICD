pipeline {
    agent {
        label 'agent-1'
    }
    options {
                // Timeout counter starts BEFORE agent is allocated
                timeout(time: 15, unit: 'MINUTES')
            }
    stages {
        stage('Build') {
            steps {
                sh 'echo this is build'
            }
        }
        stage('Test') {
            steps {
                sh 'echo this is test'
                sh 'sleep 10'
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo this is deply'
            }
        }
    }
}