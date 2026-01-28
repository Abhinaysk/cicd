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
                        url: 'https://github.com/Abhinaysk/jenkins_cicd/maven.git'
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
                
                  sh 'mvn clean compile'
                
            }
        }

        stage("SonarQube Analysis") {
            steps {
                dir('backend') {
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


// pipeline{
//     agent any
//     tools {
//         maven "maven_main"
//     }
//     stages{
//         stage("CHECKOUT"){
//             steps{
//                 checkout([
//                     $class: 'GitSCM',
//                     branches: [[name: '*/main']],
//                     userRemoteConfigs: [[
//                         credentialsId: 'github',
//                         url: 'https://github.com/Abhinaysk/jenkins_cicd/maven.git'
//                     ]]
//                 ])
//             }
//         }

//         stage("Maven Build") {
//             steps {
//                 sh """
//                     mvn clean compile
//                 """
//             }
//         }

//         stage("SONAR_SCANNING") {
//             steps {
//                 withCredentials([string(credentialsId: 'sonar-server', variable: 'SONAR_TOKEN')]) {
//                     sh '''
//                         pwd
//                         ls -lrt  
//                         sonar-scanner -Dsonar.host.url=http://35.154.206.235:9000
//                     '''
//                 }
//             }
//         }
//     }
//     post {
//         always {
//             cleanWs()
//         }
//     }
// }