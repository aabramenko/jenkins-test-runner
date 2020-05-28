pipeline {
	agent any
	stages {
		stage("Pull Auxiliary Images") {
			steps {
			    sh "docker pull selenoid/firefox:76.0"
			    sh "docker pull selenoid/chrome:83.0"
			    sh "docker pull selenoid/video-recorder:latest-release"
			}
		}
		stage("Pull Latest Test Image") {
			steps {
				sh "docker pull alekseiabramenko/selenium-test-environment"
			}
		}
		stage("Start Selenoid") {
			steps {
				sh "docker-compose -f selenoid.docker-compose.yaml up -d selenoid selenoid-ui"
			}
		}
		stage("Run Test") {
			steps {
				sh "docker-compose -f selenoid.docker-compose.yaml up test-module"
			}
		}
	}
	post {
		always {
			archiveArtifacts artifacts: 'output/**'
			sh "docker-compose -f selenoid.docker-compose.yaml down"
			publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'output/test-results/surefire-reports/html', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
			// allure includeProperties: false, jdk: '', results: [[path: 'output/test-results/allure-results']]
		}
	}
}