pipeline {
    agent any
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '8090', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '9090', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
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
                        sh "scp -i /Users/radhu/downloads/ssh/MyEC2Keypair.pem **/target/*.war ec2-user@52.201.56.27:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/radhu/downloads/ssh/MyEC2Keypair.pem **/target/*.war ec2-user@34.207.147.101:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}