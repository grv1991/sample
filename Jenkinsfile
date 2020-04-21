pipeline {
    agent any
    options {
     timestamps()
    }
    parameters { 
         string(name: 'tomcat_prod', defaultValue: '13.235.73.196', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }

stages{
        stage('Build'){
            steps {
                git credentialsId: 'a4dfee7c-ffe5-44c1-b898-1aec4583015b', url: 'https://github.com/grv1991/sample.git'
                sh 'mvn clean install package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
            stage ("Deploy to Production"){
              steps{
                sshagent(['tomcat-dev']) {
                sh 'ssh -i /home/Devops-Project.pem **/target/*.war ec2-user@${params.tomcat_prod}:/usr/local/tomcat9/webapps'
            }
        }
    }
}
}
