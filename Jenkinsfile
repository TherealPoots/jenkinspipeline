pipeline {
    agent any
    
    parameters { 
         string(name: 'ApplicationServer2', defaultValue: '54.234.229.192', description: 'Staging Server')
      //   string(name: 'tomcat_prod', defaultValue: '34.209.233.6', description: 'Production Server')
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
                        sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/opt/lib/tomcat/webapps"
                    }
                }
            }  
        }
    }
}
