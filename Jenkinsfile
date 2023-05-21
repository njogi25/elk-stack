node {
    def application = "springbootapp"
    def dockerhubaccountid = "varunmanik"
    def app  // defining app variable here to be globally accessible

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
    }

    stage('Push image') {
        withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
            app.push()
            app.push("latest")
        }
    }

    stage('Deploy') {
        sh ("docker stop ${application} || true && docker rm ${application} || true")
        sh ("docker run -d --name ${application} -p 81:8080 -v /var/log/:/var/log/ ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
    }
    
    stage('Remove old images') {
        // remove docker old images
        sh("docker image prune -f")
    }
}
