pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo '01 Hello World2 change'
            }
        }
        stage('Build') {
            steps {
                echo '02 Build'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding', 
                    credentialsId: 'jenkins_user',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]){
                    sh[scriot: 'aws s3 cp /var/lib/jenkins/workspace/pipeLine/index.html s3://deploybucket20240604']
                }
            }
        }
        stage('Test') {
            steps {
                echo '04 Test'
            }
        }
        stage('Release') {
            steps {
                echo '05 Release'
            }
        }
    }
}
