pipeline {
    agent any

    /* parameters {
         string(name: 'tomcat_dev', defaultValue: '192.168.14.133:8888', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '192.168.14.133:9999', description: 'Production Server')
    } */

    triggers {
         pollSCM('* 9 * * *')
     }

    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deploy to Staging'){
            steps {
                build job: 'Deploy-To-Staging'
            }
        }

            stage ("Deploy to Prod"){
                steps {
                    timeout(time:5, unit:'DAYS'){
                        input message: 'Approve Production Deployment?'
                    }
                    build job: 'Deploy-To-Prod'
                }
            }
    }
}

