pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '54.236.44.46', description: 'Staging Server')
      //   string(name: 'tomcat_prod', defaultValue: '34.209.233.6', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }
   
     tools {
        maven '3.6.3'
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
                       sudo sh "scp -i /home/ec2-user/JenkinsOct20.pem **/target/*.war ec2-user@${params.tomcat_dev}:/opt/lib/tomcat/webapps"
                    }
                }
            }  
        }
    }
}
