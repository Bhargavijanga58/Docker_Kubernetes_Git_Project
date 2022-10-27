
pipeline{
    agent any
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}
  stages{
    stage('Build') {
      steps {
	sh 'rm -rf *.var'
        sh 'jar -cvf mypart2project.war -C src/main/webapp .'      
        sh 'docker build -t nidhish98/studentsurvey645:latest .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
       }
    }
    stage("Push image to docker hub"){
      steps {
        sh 'docker push nidhish98/studentsurvey645:latest'
      }
    }
    stage("Deploy image to deployment in cluster"){
      steps {
        sh 'kubectl set image deployment/hello-app studentsurvey645=nidhish98/studentsurvey645:latest -n jenkins-pipeline'
      }
    }  
  }
  post {
	  always {
			sh 'docker logout'
		}
	}    
}
