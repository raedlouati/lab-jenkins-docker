pipeline{
	agent any

	stages{
		stage('Build'){
			steps {
				echo '###### This Is The Build Stage ######'
				sh 'sudo cd /root/lab-jenkins-docker'
				sh 'sudo jar -cvf dist/lab-jenkins.war src/index.html'
				}
					}
		stage('Deploy'){
			steps {
				echo 'This is the Deploy Stage'
				sh 'echo Deploy'
				}
				}
		stage('Production Deploy'){
			when {
				branch 'master'
				}
			steps{
				echo 'This is PROD'
				sh ' hostname '
				}
					}
		}


	}
