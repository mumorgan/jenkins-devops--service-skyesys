// Scripted PipeLline (dated Jenkins pipeline approach)

// Declarative Pipeline
pipeline {
	agent any
	
	environment {
		dockerHome = tool "skyesys-docker"
		mavenHome = tool "skyesys-maven"
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}

	stages {
		stage('Build') {
			steps {
				sh 'mvn --version'
				sh 'docker --version'
				echo "Build"
				echo "PATH ~ $PATH"
				echo "BUILD_NUMBER ~ $env.BUILD_NUMBER"
				echo "BUILD_ID ~ $env.BUILD_ID"
				echo "JOB_NAME ~ $env.JOB_NAME"
				echo "BUILD_TAG ~ $env.BUILD_TAG"
				echo "BUILD_URL ~ $env.BUILD_URL"
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

