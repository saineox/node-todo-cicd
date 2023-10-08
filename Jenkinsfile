pipeline {
	agent any
	stages{
		stage("code"){
			steps{
				echo "code clone"
				git url: "https://github.com/saineox/node-todo-cicd.git",branch:"master"
			}
		}

		stage("Build & Test"){
			steps{
				sh "docker build . -t node-app"
			}
		}

		stage("Push to repository"){
			steps{
			     withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-app ${env.dockerHubUser}/node-app-jenkins:latest"
                    sh "docker push ${env.dockerHubUser}/node-app-jenkins:latest" 
                }
				echo "push docker hub"
			    
			}
		}

		stage("Deploy"){
			steps{
				echo "Deploy Aws start "
                sh "docker-compose down"
				sh "docker-compose up -d"
				echo "Deploy Aws finished"
			}
		}
	}
}
