pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '34.235.142', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.87.133.45', description: 'Production Server')
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /root/tomcatDemo.pem **/target/*.war ubuntu@${params.tomcat_dev}:/var/lib/tomcat9/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /root/tomcatDemo.pem **/target/*.war ubuntu@${params.tomcat_prod}:/var/lib/tomcat9/webapps"
                    }
                }
            }
        }
    }
}