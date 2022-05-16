node {
  
  environment {
	DOCKERHUB_CREDENTIALS=credentials('DockerHub Credentials')
  }
  
  stage('Checkout Source Code') {
    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GitHub Credential', url: 'https://github.com/kevinchoy-me/php_project.git']]])
  }

  stage('Create Docker Image') {
    docker.build("docker_image:${env.BUILD_NUMBER}")
  }
  
  stage('Upload Docker Image to Docker Hub')
  {
	env_build_number = $env.BUILD_NUMBER
	withDockerRegistry(credentialsId: 'DockerHub Credentials') {
		sh "docker tag docker_image:$env_build_number kevinchoy007/docker_image:$env_build_number"
		sh "docker push kevinchoy007/docker_image:$env_build_number"
	}

  }

  stage ('Run Application') {
    try {
      // Stop existing Container
	  sh 'docker pull kevinchoy007/docker_image:${env.BUILD_NUMBER}'
      sh 'docker rm php_project_container -f'
      // Start database container here
      sh "docker run -d --name php_project_container -p 80:80 docker_image:${env.BUILD_NUMBER}"
    } 
	catch (error) {
    } finally {
      // Stop and remove database container here
    }
  }
  
  stage ('Notifications') {
        // Notification
     }
 }