pipeline {
    agent { label 'workder1' }	

    tools {
        maven "maven_3.6.3"
    }

	environment {	
		DOCKERHUB_CREDENTIALS=credentials('dockerhub_access')
	} 
    
    stages {
        stage('SCM Checkout') {
            steps {
                git 'https://github.com/masun11/star-agile-banking-finance.git'
            }
		}
        stage('Maven Build') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
		}
       stage("Docker build"){
            steps {
				sh 'docker version'
				sh "docker build -t mausnulla/bankapp-eta-app:${BUILD_NUMBER} ."
				sh 'docker image list'
				sh "docker tag mausnulla/bankapp-eta-app:${BUILD_NUMBER} mausnulla/bankapp-eta-app:latest"
            }
        }
		stage('Login2DockerHub') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}
		stage('Push2DockerHub') {

			steps {
				sh "docker push mausnulla/bankapp-eta-app:latest"
			}
		}
    }
}
