pipeline {
    agent { label 'dev' }
        
    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/Tharun4153/Project-on-Building_and_Deploying-a-Node.js-Application-with-Docker-on-Ubuntu.git',
				branch: 'master'
            }
        }
        stage('Build'){
            steps{
                sh 'docker build . -t tharun4153/nodo-todo-app-test:latest'
            }
        }
        stage('Test image') {
            steps {
                echo 'testing…'
                sh 'docker image inspect tharun4153/nodo-todo-app-test:latest'
            }
        }
        stage('Push') {
			steps{
				sh "sudo docker login -u tharun4153 -p dckr_pat_rawtlhzz29-r4dLjfGCQLhXBrkM"
				sh 'sudo docker push tharun4153/nodo-todo-app-test:latest'
			}
		}
		stage('Deploy') {
            steps{
				echo 'deploying on another server'
				sh 'sudo docker stop nodetodoapp || true'
				sh 'sudo docker rm nodetodoapp || true'
				sh 'sudo docker run -d --name nodetodoapp -p 8000:8000 tharun4153/nodo-todo-app-test:latest'
				sshagent(credentials: ['10.0.0.41']) {
				sh '''
					ssh -o StrictHostKeyChecking=no ubuntu@10.0.0.41 <<EOF
					sudo docker login -u tharun4153 -p dckr_pat_rawtlhzz29-r4dLjfGCQLhXBrkM
					sudo docker pull tharun4153/nodo-todo-app-test:latest
					sudo docker stop nodetodoapp || true
					sudo docker rm nodetodoapp || true
					sudo docker run -d --name nodetodoapp -p 8000:8000 tharun4153/nodo-todo-app-test:latest
                					'''
                		    
			}    
        	}
        }

    }
}
