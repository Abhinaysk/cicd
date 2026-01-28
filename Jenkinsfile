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
            ls -lrt
            find . -name pom.xml
            '''
       }
    }


        stage("Maven Build") {
            steps {
                dir('./maven-project'){
                  sh '''
                    cd maven-project 
                    mvn clean compile
                    '''
                }
            }
        }

        stage("SonarQube Analysis") {
            steps {
                dir('./maven-project') {
                    withCredentials([
                        string(credentialsId: 'sonar-scanner', variable: 'SONAR_TOKEN')
                    ]) {
                        sh """
                        mvn sonar:sonar \
                        -Dsonar.host.url=http://13.201.32.81:9000 \
                        -Dsonar.login=$SONAR_TOKEN
                        """
                    
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
