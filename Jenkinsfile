pipeline {
    agent {
        label 'agent-1'
    }
    options {
                // timeout(time: 100, unit: 'SECONDS')
                timeout(time: 15, unit: 'MINUTES')
                disableConcurrentBuilds()
                ansiColor('xterm')
            }
    environment {
        def appVersion = ''
    }
    stages {
        stage('read the version'){
            steps {
                script {
                def packageJson = readJSON file: 'package.json'
                def appVersion = packageJson.version
                echo "application version: $appVersion"
             }
            }
        }
        stage('Install dependencies') {
            steps {
                sh """
                    npm install
                    ls -ltr
                    echo "application version: $appVersion"
                """
            }
        }
 
    }
    post { 
            always { 
                echo 'I will always say Hello again!'
                deleteDir()
            }
            success { 
                echo 'I will run when pipeline sucess'
            }
            failure { 
                echo 'I will run when pipeline failure'
            }
        } 
}
