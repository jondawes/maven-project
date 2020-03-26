pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '34.235.142.7', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.87.133.45', description: 'Production Server')
         string(name: 'ssh_key', defaultValue: '/var/lib/jenkins/keys/tomcatDemo.pem', description: 'SSH Key location for deployment servers')
    }

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

        stage ('Testing'){            parallel{
                stage ( 'Static Analysis'){
                    steps {
                        sh mvn 'checkstyle:checkstyle'

                        //def checkstyle = scanForIssues tool: [$class: 'CheckStyle'], pattern: '**/target/checkstyle-result.xml'
                        //publishIssues issues:[checkstyle]
                    }
                }

                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i ${params.ssh_key} **/target/*.war ubuntu@${params.tomcat_dev}:/var/lib/tomcat9/webapps"
                    }
                }
            }
        }

        stage ("Deploy to Production"){
            steps {
                sh "scp -i ${params.ssh_key} **/target/*.war ubuntu@${params.tomcat_prod}:/var/lib/tomcat9/webapps"
            }
        }

    }
}

