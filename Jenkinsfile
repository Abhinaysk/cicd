pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {

        stage("Checkout Code") {
            steps {
                git(
                  url: 'https://github.com/Abhinaysk/jenkins_cicd.git',
                  branch: 'main',
                  credentialsId: 'git_hub'
                )
            }
        }

        stage("Debug Workspace") {
            steps {
                sh '''
                pwd
                ls -R .
                find . -name pom.xml
                '''
            }
        }

        stage("Maven Build") {
            steps {
                dir('maven-project') {
                    sh 'mvn clean compile'
                }
            }
        }

        stage("SonarQube Analysis") {
            steps {
                dir('maven-project') {
                    withCredentials([string(credentialsId: 'sonar-backend', variable: 'SONAR_TOKEN')]) {
                        sh '''
                        mvn sonar:sonar \
                          -Dsonar.host.url=http://13.127.143.96:9000 \
                          -Dsonar.login=$SONAR_TOKEN
                        '''
                    }
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
