pipeline {
    agent any

    tools {
        maven 'maven'   // Must be configured in Global Tool Configuration
    }

    stages {

        stage("CHECKOUT") {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        credentialsId: 'git_hub',
                        url: 'https://github.com/Abhinaysk/jenkins_cicd.git'
                    ]]
                ])
            }
        }

        stage("Maven Build") {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage("SonarQube Analysis") {
            steps {
                withCredentials([
                    string(credentialsId: 'sonar_scanner', variable: 'SONAR_TOKEN')
                ]) {
                    sh """
                    mvn sonar:sonar \
                      -Dsonar.projectKey=jenkins-cicd \
                      -Dsonar.host.url=http://35.154.94.205:9000/ \
                      -Dsonar.login=$SONAR_TOKEN
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
