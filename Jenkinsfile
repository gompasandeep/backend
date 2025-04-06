pipeline {
    agent {
        label 'AGENT-1'
    }

    options {
        timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds()
        //retry(1)
    }
    environment {
        DEBUG = 'true'
        appVersion = '' // this will become global we can use across pipeline
    }
    stages {
        stage('Read the version') {
            steps{
                script{
                def packageJson = readJSON file: 'package.json'
                appVersion = packageJson.version
                echo "App version: ${appVersion}"
            }

            } 
        }

        stage('Test') {
            steps {
                sh 'echo This is test'
                sh 'env'
            }
        }

        stage('Deploy') {
            when{
                expression { env.GIT_BRANCH == "origin/main" }
            }
            steps {
                sh 'echo "This is Deploy"'
               // error "pipeline failed"
            }
        }
    }

    post {
        always {
            echo "This section runs always"
            deleteDir()
        }
        success {
            echo "This section runs when pipeline succeeds"
        }
        failure {
            echo "This section runs when pipeline fails"
        }
    }
}
