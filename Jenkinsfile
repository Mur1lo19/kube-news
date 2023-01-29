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
		stage ('push docker image') {
			steps  {
				script {
					docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
						dockerapp.push('latest')
						dockerapp.push("${env.BUILD_ID}")
					}
					
				}
			}
		}
		stage ('deploy kubernetes') {
			environment {
				tag_version = "${env.BUILD_ID}"
			}
			steps {
				withKubeConfig ([credentialsId: 'kubeconfig']) {
					sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
					sh 'kubectl apply -f k8s/deployment.yaml'
				}
			}
		}

	}
}


