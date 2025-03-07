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
					sh 'mvn sonar:sonar -Dsonar.login=${SONAR_TOKEN}'
                }
            }
        }
        stage('Quality Gate') {
			steps {
				script {
					timeout(time: 5, unit: 'MINUTES') {
						waitForQualityGate abortPipeline: true
					}
                }
            }
        }
        stage('Upload to Nexus') {
			steps {
				script{
					nexusArtifactUploader artifacts:
							[[artifactId: 'Jenkins_Sonarqube',
							classifier: '',
							file: 'target/Jenkins_Sonarqube-0.0.1-SNAPSHOT.jar',
							type: 'jar']],

							credentialsId: 'nexus-cred',
							groupId: 'com.example',
							nexusUrl: 'localhost:8081',
							nexusVersion: 'nexus3',
							protocol: 'http',
							repository: 'maven-snapshots',
							version: '0.0.1-SNAPSHOT'
				}
            }
        }

    }
}
