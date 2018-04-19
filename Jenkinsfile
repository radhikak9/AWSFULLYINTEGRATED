pipeline {
    agent any
      tools {
    maven 'localMaven'
  }
    parameters { 
         string(name: 'tomcat_dev', defaultValue: 'localhost:8090', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost:9090', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control test
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
                stage ('deploy-to-staging'){
                    steps {
                        sh "cp -i /Users/radhu/downloads/ssh/MyEC2Keypair.pem **/target/*.war ec2-user@52.201.56.27:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("deploy-to-production"){
                    steps {
                        sh "cp -i /Users/radhu/downloads/ssh/MyEC2Keypair.pem **/target/*.war ec2-user@34.207.147.101:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}