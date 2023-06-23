pipeline {
    agent { label 'dev' }
        
    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://git@github.com:ick18/Project-on-Building-and-Deploying-a-Node.js-Application-with-Docker-on-Ubuntu.git',
				branch: 'main'
            }
        }
        stage('Build'){
            steps{
                sh 'docker build . -t ckvk18/node-todo-app:latest'
            }
        }
        stage('Test image') {
            steps {
                echo 'testing…'
                sh 'docker image inspect ckvk18/node-todo-app:latest'
            }
        }
        stage('Push') {
			steps{
				sh "sudo docker login -u ckvk18 -p dckr_pat_g9iJMSZYwOKQfJbs6IXKkkl2Rs8"
				sh 'sudo docker push ckvk18/node-todo-app-test:latest'
			}
		}
		stage('Deploy') {
            steps{
				echo 'deploying on another server'
				sh 'sudo docker stop nodetodoapp || true'
				sh 'sudo docker rm nodetodoapp || true'
				sh 'sudo docker run -d --name nodetodoapp -p 8000:8000 ckvk18/node-todo-app-test:latest'
				sshagent(credentials: ['10.0.0.51']) {
				sh '''
					ssh -o StrictHostKeyChecking=no ubuntu@10.0.0.51 <<EOF
					sudo docker login -u ckvk18 -p dckr_pat_rawtlhzz29-r4dLjfGCQLhXBrkM
					sudo docker pull ckvk18/node-todo-app-test:latest
					sudo docker stop nodetodoapp || true
					sudo docker rm nodetodoapp || true
					sudo docker run -d --name nodetodoapp -p 8000:8000 ckvk18/node-todo-app-test:latest
                					'''
                		    
			}    
        	}
        }

    }
}
