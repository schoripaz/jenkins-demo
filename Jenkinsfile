pipeline {
    agent any

    /* parameters {
         string(name: 'tomcat_dev', defaultValue: '192.168.14.133:8888', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '192.168.14.133:9999', description: 'Production Server')
    } */

    triggers {
         pollSCM('* * * * *')
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        Build job: 'Deploy-To-Staging'
                    }
                   /*  {
                        sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    } */
                }

                stage ("Deploy to Prod"){
                    steps {
                        timeout(time:5, unit:'DAYS'){
                            input message: 'Approve Production Deployment?'
                        }
                        Build job: 'Deplo-To-Prod'
                    }
                    /*{
                        sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    } */
                }
            }
        }
    }
}
