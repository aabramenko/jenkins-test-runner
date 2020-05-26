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
		stage("Allure report") {
		    steps {
			    script {
				    allure([
					    includeProperties: false,
					    jdk: '',
					    properties: [],
					    reportBuildPolicy: 'ALWAYS',
					    results: [[path: 'target/allure-results']]
				    ])
			    }
		    }
		}		
	}
	post {
		always {
			archiveArtifacts artifacts: 'output/**'
			sh "docker-compose -f hub.docker-compose.yaml down"
		}
	}
}
