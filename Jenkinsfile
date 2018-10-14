cd5107a04baf92e907afa0f4c0cc33d57b362b53

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
		stage ('DeployToStaging') {
		    when {
			    branch 'master'
			}
			steps {
			    withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
				sshPublisher(
				    failOnError: true,
					continueOnError: false,
					publisher: [
					configName: 'staging'
					sshCredentials: [
					    username: '$USERNAME',
						encryptedPassPhrase: '$USERPASS'
					],
					transfers: [
					sshTransfer(
					    sourceFiles: 'dist/trainSchedule.zip',
						removePrefix: 'dist/',
						remoteDirectory: '/temp',
						execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /temp/train-schedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
						)
					]
				)
				]
				}
			}
		
		}
    }
}

























