pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo '01 Hello Bunkakai'
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
                    sh(script: 'aws s3 cp /var/lib/jenkins/workspace/pipeLine/index.html s3://deploybucket20240604/')
                }
            }
        }
        stage('Test') {
            steps {
                echo '04 Test'
                script {
                    def url = 'https://deploybucket20240604.s3.ap-northeast-1.amazonaws.com/index.html'
                    def response = sh(script: "curl -s -o /dev/null -w '%{http_code}' '$url'", returnStdout: true)

                    if (response == '200') {
                        echo 'Test OK'
                    } else {
                        echo response
                        error 'Test NG'
                    }
                }
            }
        }
        stage('Release') {
            steps {
                echo '05 Release'
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'jenkins_user',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]){
                        sh(script: 'aws s3 cp /var/lib/jenkins/workspace/pipeLine/index.html s3://releasebucket20240604/')
                }
            }
        }
    }
}
