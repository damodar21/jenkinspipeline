pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '54.208.9.133', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.208.230.118', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh '/home/damodar/Downloads/apache-maven-3.6.3/bin/mvn clean package'
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
                        sh "scp -o 'StrictHostKeyChecking no' -i /home/damodar/Downloads/LinuxmachineKeypair.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -o 'StrictHostKeyChecking no' -i /home/damodar/Downloads/LinuxmachineKeypair.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}