    pipeline {
        agent any
     
        tools {
            maven 'localMaven'
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
            stage('Deploy to staging'){
                steps {
                build job: 'deploytostaging'
                }
           }
           stage('Deploy to Production'){
              
                steps {
                    timeout(time:5, unit:'DAYS'){
                    input message:'Approve Production deployment?'
               }
                build job: 'deploy-to-prod'
                }
            post{
                success {
                    echo 'Code deployed to production'
                }
                failure {
                    echo 'Deployment failed .'
                }
            }
           }
        }
    }