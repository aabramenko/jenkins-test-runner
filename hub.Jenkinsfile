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
				sh "docker-compose -f hub.docker-compose.yaml up -d hub chrome firefox"
			}
		}
		stage("Run Test") {
			steps {
				sh "docker-compose -f hub.docker-compose.yaml up test-module"
			}
		}
	}
	post {
		always {
			archiveArtifacts artifacts: 'output/**'
			sh "docker-compose -f hub.docker-compose.yaml down"
			publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'output/test-results/surefire-reports/html', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
			// allure includeProperties: false, jdk: '', results: [[path: 'output/test-results/allure-results']]
		}
	}
}
