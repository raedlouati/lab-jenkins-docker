pipeline{
	agent any

	stages{
		stage('Build'){
			steps {
			withCredentials ([usernamePassword(credentialsId: 'deploy', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
				echo '###### This Is The Build Stage ######'
sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.dev_ip} \" cd /home/deploy/lab-jenkins-docker/src ; jar -cvf ../dist/lab-jenkins.war index.html \""
sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.dev_ip} \" cd /home/deploy/lab-jenkins-docker ; docker build -t rlouati/lab-jenkins:latest .   \""
 echo 'Build docker image complete'
sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.dev_ip} \" cd /home/deploy/lab-jenkins-docker ; docker push rlouati/lab-jenkins:latest   \""

				}
			}
					}
					
		stage('Deploy'){
			steps {
			echo 'This is the Deploy Stage'
			withCredentials ([usernamePassword(credentialsId: 'deploy', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
			script {
			try {
				sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.dev_ip} \" docker stop lab-jenkins \"" ;
				sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.dev_ip} \" docker rm lab-jenkins \"" ;
				} catch (err) {
					echo :' Caught error : $err' ;
					}
}
			sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.dev_ip} \" docker run --restart always -dp 7070:8080 --name lab-jenkins rlouati/lab-jenkins:latest \""



										}
}

				}

		
	                stage('DEPLOY TO PROD'){
                        steps {
                        input 'DO YOU CONFIRM THE DEPLOYMENT TO THE PROD ?'

                        withCredentials ([usernamePassword(credentialsId: 'deploy', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                        script {
                        try {
                                sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.prod_ip} \" docker stop lab-jenkins \"" ;
                                sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.prod_ip} \" docker rm lab-jenkins \"" ;
                                } catch (err) {
                                        echo :' Caught error : $err' ;
                                        }
}
                        sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.prod_ip} \" podman rmi docker.io/rlouati/lab-jenkins -f ; docker run --restart always -dp 8080:8080 --name lab-jenkins rlouati/lab-jenkins:latest \""



                                                                                }
}

                                }


}


	}
