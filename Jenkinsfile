node {
  
  stage('Checkout Source Code') {
    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GitHub Credential', url: 'https://github.com/kevinchoy-me/php_project.git']]])
  }

  stage('Create Docker Image') {
    docker.build("docker_image:${env.BUILD_NUMBER}")
  }

  stage ('Run Application') {
    try {
      // Stop existing Container
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