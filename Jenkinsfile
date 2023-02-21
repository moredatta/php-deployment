pipeline {
  agent any
   tools {
    maven 'M3'
  }
  environment {
    CLOUDSDK_CORE_PROJECT='capable-sphinx-378108'
    CLIENT_EMAIL='jenkins@capable-sphinx-378108.iam.gserviceaccount.com'
    GCLOUD_CREDS=credentials('gcloud-creds')
    DOCKERHUB_CREDENTIALS = credentials('docker')
  	}
  stages {
	    stage('Verify version') {
	      steps {
		sh '''
		  gcloud version
		'''
	      }
	    }
	    stage('Authenticate') {
	      steps {
		sh '''
		  gcloud auth activate-service-account --key-file="$GCLOUD_CREDS"
		'''
	      }
	    }
	    stage('Login') {
			    steps {
		      sh "echo $DOCKERHUB_CREDENTIALS_USR"
		   sh "echo $DOCKERHUB_CREDENTIALS_PSW"
				sh  "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW"
				}
			}
		
	      stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0';
        	steps{
        	withSonarQubeEnv('sonarqube-9.8') { 
        // If you have configured more than one global server connection, you can specify its name
	   sh "${scannerHome}/bin/sonar-scanner"
       		sh '''
			sonar-scanner \
 			 -Dsonar.projectKey=smartsuite \
  			 -Dsonar.sources=. \
 			 -Dsonar.host.url=http://34.100.210.20:9000 \
 			 -Dsonar.login=sqp_6a1c4a6b7cf1848677164e0904d4f92a0c5654bf
		'''
    }
        }
        }
	  
	    stage('pull'){
		    steps{
		       sh "docker pull  moredatta574/jenkins-php"
		    }
		 }
	       stage('tag'){
		    steps{
		       sh "docker tag  moredatta574/jenkins-demo gcr.io/capable-sphinx-378108/moredatta574/jenkins-php"
		    }
		 }
	    
	     stage('list'){
		    steps{
		       sh "gcloud container images list --repository=gcr.io/capable-sphinx-378108"
		    }
		 }


	    stage('Install service') {
	      steps {
		sh '''
		  gcloud run services replace service.yaml --platform='managed' --region='asia-south1'
		'''
	      }
	    }
	    stage('Allow allUsers') {
	      steps {
		sh '''
		  gcloud run services add-iam-policy-binding hello-php --region='asia-south1' --member='allUsers' --role='roles/run.invoker'
		'''
	      }
	    }
  }
  post {
    always {
      sh 'gcloud auth revoke $CLIENT_EMAIL'
    }
  }
}
