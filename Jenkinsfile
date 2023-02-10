pipeline{
    agent any
    tools {
    maven 'M3'
  }
    environment {
	    	
		DOCKERHUB_CREDENTIALS = credentials('docker')
	}
    stages {
         stage('Login') {
		    steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_USR'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW'
		        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW'
			}
		}
       
       
        stage('Build Image') {
            steps {
                sh "docker build -t moredatta574/php ."
            }
        }
        stage('push Image') {
            steps {
                sh "docker push  moredatta574/php"
            }
        }
	    
        
        stage('Test') {
            steps {
                echo 'Successfully Pushed'
            }
        }
