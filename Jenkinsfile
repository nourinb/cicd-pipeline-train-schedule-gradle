pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainschedule.zip'
            }
        }
        stage('DeployToStaging'){
            steps {
                withCredentials([usernamePassword(credentialsId:'webservergithubapikey',usernameVariable: 'USERNAME',passwordVariable: USERPASS)]){ 
                    sshPublisher{
                    failOnErrror: true,
                    continueOnError: false,
                    publishers: [
                        sshPublisherDesc{
                         configName: 'staging'
                         sshCredentials: [
                             username: "$USERNAME"
                             encryptedPassPhrase: "$USERPASS"
                             ],
                             transfers: [
                                 sshTransfer{
                                 sourceFiles: 'dist/trainschedule.zip',
                                 removePrefix: 'dist/',
                                 remoteDirectory: '/tmp',
                                 execCommand: 'sudo unzip /tmp/trainschedule.zip -d /opt '  
                                 }
                              ]
                           }
                        ]
                    }
                }
            }
        }
    }

