pipeline {
	agent any
	tools {
		maven 'Maven'
	}
    environment {
		SONAR_SCANNER_HOME = tool 'SonarQube-Scanner'
    }
    stages {
		stage('Clone') {
			steps {
				git 'https://github.com/Minhhieunekk/Jenkins.git'
            }
        }
        stage('Build & Test') {
			steps {
				sh 'mvn clean verify'
            }
        }
        stage('SonarQube Analysis') {
			steps {
				withSonarQubeEnv('SonarQube') {
					sh 'mvn sonar:sonar -Dsonar.login=sqa_02e4c30eb9fd4ea9ebc89decbb80f8811181a9d1'
                }
            }
        }
        stage('Quality Gate') {
			steps {
				script {
					waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
