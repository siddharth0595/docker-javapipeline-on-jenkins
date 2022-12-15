pipeline {
	agent {	
		label 'slave1'
		}
	stages {
		stage("SCM") {
			steps {
				git branch: 'main', url: 'https://github.com/siddharth0595/docker-javapileline-on-jenkins.git'
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
				sh 'sudo docker tag java-repo:$BUILD_TAG siddharth121/pipeline-java:$BUILD_TAG'
				}
			}
				
	
		stage("Docker Hub") {
			steps {
			withCredentials([string(credentialsId: 'docker_hub', variable: 'docker_hub_password_var')])   {
				sh 'sudo docker login -u siddharth121 -p ${docker_hub_password_var}'
				sh 'sudo docker push siddharth121/pipeline-java:$BUILD_TAG'
				}
			}
		}

		stage("QAT Testing") {
			steps {
				sh 'sudo docker run -dit -p 8081:8080 --name web13 siddharth121/pipeline-java:$BUILD_TAG'
				}
			}
	 	stage("testing website") {
			steps {
				retry(5) {
				sh "curl --silent http://35.154.193.35:49153/java-web-app/ | grep -i india"
				}
	   		}
		}

		stage("Approval status") {
			steps {
				script {  
                		Boolean userInput = input(id: 'Proceed1', message: 'Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
                		echo 'userInput: ' + userInput
				}
	 		}
		}
    	}
}
