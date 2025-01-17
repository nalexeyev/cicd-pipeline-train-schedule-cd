pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('DeployStaging') {
            steps {
                echo 'Running deploy automation'
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'Staging',
                            transfers: [
                                sshTransfer(
                                        sourceFiles: 'dist/trainSchedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                                )
                            ],
                        )
                    ]
                )
            }
        }
        stage('DeployProd') {
            steps {
                echo 'Running deploy automation'
                input 'Deploy?'
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'Prod',
                            transfers: [
                                sshTransfer(
                                        sourceFiles: 'dist/trainSchedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                                )
                            ],
                        )
                    ]
                )
            }
        }
    }
}
