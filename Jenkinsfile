pipeline {
	agent {	
		label 'jenkins-slave2'
		}
	stages {
		stage("SCM") {
			steps {
			        git https://github.com/siddharth0595/docker-javapileline-on-jenkins.git
				}
			}

		stage("build") {
			steps {
				sh 'sudo mvn dependency:purge-local-repository'
				sh 'sudo mvn clean package'
				}
			}
		stage("Image") {
			steps {
				sh 'sudo docker build -t java-repo:$BUILD_TAG .'
				sh 'sudo docker tag java-repo:$BUILD_TAG srronak/pipeline-java:$BUILD_TAG'
				}
			}
				
	
		stage("Docker Hub") {
			steps {
			withCredentials([string(credentialsId: 'docker_hub_passwd', variable: 'docker_hub_password_var')]) {
				sh 'sudo docker login -u srronak -p ${docker_hub_password_var}'
				sh 'sudo docker push srronak/pipeline-java:$BUILD_TAG'
				}
			}	

		}
		stage("QAT Testing") {
			steps {
				sh 'sudo docker run -dit -p 8080:8080 --name web11 srronak/pipeline-java:$BUILD_TAG'
				}
			}
	}
}

