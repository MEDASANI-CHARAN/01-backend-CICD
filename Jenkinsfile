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
        nexusUrl = 'jenkins-nexus.daws2025.online:8081'
    }
    stages {
        stage('read the version'){
            steps {
                script {
                def packageJson = readJSON file: 'package.json'
                appVersion = packageJson.version
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
        stage('Build') {
            steps {
                sh """
                    zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                    ls -ltr
                """
            }
        }

        stage('Nexus Artifact Upload') {
            steps {
               script {
                    nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusUrl}",
                    groupId: 'com.expense',
                    version: "${appVersion}",
                    repository: 'backend',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: 'backend',
                        classifier: '',
                        file: 'backend-' + "${appVersion}" + '.zip',
                        type: 'zip']
                    ]
                )
             }
          }
        }
        stage('Deploy'){
            steps {
                     def params = [
                        string(name: 'appVersion', value: "${appVersion}")
                    ]
                    script {
                         build job: '01-backend-deployment', parameters: params, wait: false
                    }
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
