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
        PROJECT = 'expense'
        COMPONENT = 'backend'
        DEBUG = 'true'
        appVersion = '' // this will become global we can use across pipeline
        ACC_ID =  339713057882
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

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        // stage('Docker Build') {
        //     steps {
        //         sh """
        //         docker build -t sandeepgompa/backend:${appVersion} .
        //         """
        //     }
        // }
        stage('Docker Build'){
            steps {
               script{
                withAWS(region: 'us-east-1', credentials: 'aws-creds') {
                    sh """
                    aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                    
                    docker build -t  ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${appVersion} .

                    docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${appVersion}
                    """
                }
                 
               }
            }
        }
    }

    post {
        always {
            echo "This section always runs"
            deleteDir()
        }
        success {
            echo "This section runs when pipeline succeeds"
        }
        failure {
            echo "This section runs when pipeline fail"
        }
    }
}
