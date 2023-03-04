node {
	def application = "springbootapp"
	def dockerhubaccountid = "gappyduck"
	stage('Clone repository') {
		checkout scm
	}

	stage('Build image') {
		app = docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
	}

	stage('Push image') {
		withDockerRegistry([ credentialsId: "dockerHub", url: "https://index.docker.io/v1" ]) {
		app.push("${env.BUILD_NUMBER}")
		app.push("latest")
	}
	}

	stage('Deploy') {
		sh ("docker run -d -p 70:7070 -v /var/log/:/var/log/ ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
	}
	
	stage('Remove old images') {
		// remove docker pld images
		sh("docker rmi ${dockerhubaccountid}/${application}:latest -f")
   }
}
