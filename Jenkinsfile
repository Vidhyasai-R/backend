pipeline{
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        //retry(1)
    }
    environment{
        DEBUG = 'true'
        appVersion = ''
    }

    stages {
        stage('Read the version') {
            steps {
                script{
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "AppVersion: ${appVersion}"
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Docker Build') {
            steps {
                sh """
                docker build -t joindevsecops/backend:${appVersion} .
                docker images
                """
            }
        }
    }

    post {
        always{
            echo "This section runs always"
            deleteDir()
        }
    }
}