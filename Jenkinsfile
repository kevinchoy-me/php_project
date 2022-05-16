node {
  stage('Checkout Source Code') {
	// Checkout code from GitHub
    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GitHub Credential', url: 'https://github.com/kevinchoy-me/php_project.git']]])
  }

  stage('Create Docker Image') {
	// Build Docker Image
    docker.build("php_project_image:${env.BUILD_NUMBER}")
  }
  
  stage('Upload Docker Image to Docker Hub')
  {
	// Tag and push Docker Image to Docker Hub
	withDockerRegistry(credentialsId: 'DockerHub Credentials') {
		sh "docker tag php_project_image:${env.BUILD_NUMBER} kevinchoy007/php_project_image:${env.BUILD_NUMBER}"
		sh "docker push kevinchoy007/php_project_image:${env.BUILD_NUMBER}"
	}
  }

  stage ('Run Application') {
    try {
	
	// Pull Docker Image from DockerHub
	  withDockerRegistry(credentialsId: 'DockerHub Credentials') {
		sh "docker pull kevinchoy007/php_project_image:${env.BUILD_NUMBER}"
	}

      // Stop existing Container
      sh 'docker rm php_project_container -f'
      sh "docker run -d --name php_project_container -p 80:80 php_project_image:${env.BUILD_NUMBER}"
    } 
	catch (error) {
    }
  }
  
  stage ('Notifications') {
        // Notification
     }
 }