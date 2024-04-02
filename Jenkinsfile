// Scripted PipeLline (dated Jenkins pipeline approach)

// Declarative Pipeline
pipeline {
	// agent any
	agent {
		docker {
			image 'maven:3.6.3'
		}
	}
	stages {
		stage('Build') {
			steps {
				sh 'mvn --version'
				echo "Build"
			}
		}
		stage('Test') {
			steps {
				echo "Test"
			}
		}
		stage('Integration Test') {
			steps {
				echo "Integration Test"
			}
		}
	} 
	
	post {
		always {
			echo "I'm awesome, I run always"
		}
		success {
			echo "I run when pipeline is successful"
		}
		failure {
			echo "I run when pipeline fails"
		}
		changed {
			echo "I run when status of subsequent builds change"
		}
	}
}

