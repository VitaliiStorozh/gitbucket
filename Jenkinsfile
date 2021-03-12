pipeline {
    agent { label 'sbt' }

    stages {
        stage('First step') {
            steps {
                echo 'git'
                git branch: 'master', url: 'https://github.com/VitaliiStorozh/gitbucket.git'
            }
        }
        stage('Second step') {
            steps {
                echo 'build'
                sh 'sbt package'
            }
        }
        stage('Deploying') {
            steps {
                sshPublisher(
                    continueOnError: false, failOnError: true,
                    publishers: [
                        sshPublisherDesc(
                            configName: "Tomcat2",
                            verbose: true,
                            transfers: [
                                sshTransfer(execCommand: ''' cp /root/tomcat/target/scala-2.13/gitbucket_2.13-4.35.3.war /opt/tomcat/webapps/
                                    cd /opt/tomcat/bin/
				                    ./shutdown.sh
				                    ./startup.sh''', sourceFiles: "**/*.war")]
                                )
                        ]
                    )
                }
            }
    }
}
