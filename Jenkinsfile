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

    stage('Debug Workspace') {
       steps {
            sh '''
            pwd
            ls -la
            find . -name pom.xml
            '''
       }
    }


        stage("Maven Build") {
            steps {
                sh '''
                cd maven-project 
                mvn clean compile
                '''
            }
        }

        stage("SonarQube Analysis") {
            steps {
                dir 
                withCredentials([
                    string(credentialsId: 'sonar-scanner', variable: 'SONAR_TOKEN')
                ]) {
                    sh """
                    cd maven-project
                    mvn sonar:sonar \
                      -Dsonar.projectKey=jenkins-cicd \
                      -Dsonar.host.url=http://13.201.32.81:9000// \
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
