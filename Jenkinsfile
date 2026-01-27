pipeline {
    agent any
    satges{
        stage('checkout'){
            checkout{
                 $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/Abhinaysk/jenkins_cicd.git',
                        credentialsId: 'git_hub'
                    ]]
            }
        }
    }

    satges{
        stage("maven build"){
            steps{
                sh 'mvn clean compile'
            }
        }
    }

    stages {
        stage('sonar_quality') {
            steps {
                withcredentials([string(credentialsId: 'sonar_scanner', variable: 'SONAR_TOKEN')]) {
                    sh """
                    ls -lrt
                    pwd
                    -Dsonar.host.url=http://13.233.179.131:9000 \
                    """
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}