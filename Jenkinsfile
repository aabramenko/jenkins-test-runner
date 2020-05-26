pipeline {
	agent any
	stages {
		stage("Pull Latest Image") {
			steps {
				sh "docker pull alekseiabramenko/selenium-test-environment"
			}
		}
		stage("Start Grid") {
			steps {
				sh "docker-compose up -d hub chrome firefox"
			}
		}
		stage("Run Test") {
			steps {
				sh "docker-compose up test-module"
			}
		}
	}
	post {
		always {
			archiveArtifacts artifacts: 'output/**'
			sh "docker-compose down"
			sh "sudo rm -r output/"
		}
	}
}
