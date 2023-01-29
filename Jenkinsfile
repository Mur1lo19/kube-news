pipeline {
	agent any

	stages {
		stage ('build docker image') {
			steps {
				script {
					dockerapp = docker.build("muriloferreira/kube-news2023:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
				}
			}
		}

	}
}


