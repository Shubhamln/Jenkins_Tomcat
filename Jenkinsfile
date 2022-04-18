pipeline{
    agent any
    stages{
        stage("Git checkout"){
            steps{
                git branch: 'main', credentialsId: 'Github', url: 'https://github.com/Shubhamln/Jenkins_Tomcat.git'
            }
        }
        stage("Maven Build"){
            steps{
                sh "mvn clean install"
                sh "mv target/*.jar target/myapp.jar"
            }
        }
        stage("Deploy"){
            steps{
                sshagent(['SSH']) {
                    sh """
                        scp -o StrictHostKeyChecking=no target/myapp.jar ec2-user@172.31.93.120:/opt/apache-tomcat-9.0.35/webapps/
                        ssh ec2-user@172.31.93.120 sudo tomcatdown
                        ssh ec2-user@172.31.93.120 sudo tomcatup
                    """
                }    
            }
        }
    }
}
