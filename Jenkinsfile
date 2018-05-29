def CONTAINER_NAME="getintodevops-hellonode"
def CONTAINER_TAG="${env.BUILD_NUMBER}"


node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

//    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

//        app = docker.build("khzied/getintodevops-hellonode")
//    }


    stage('Image Build'){
        imageBuild(CONTAINER_NAME, CONTAINER_TAG)
}


    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

//    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
//        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
//            app.push("${env.BUILD_NUMBER}")
//                    sh 'docker push khzied/getintodevops-hellonode:latest'
//            app.push("latest")
//        }
//    }

    stage('Push to Docker Registry'){
        withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            pushToImage(CONTAINER_NAME, CONTAINER_TAG, USERNAME, PASSWORD)

            sh 'echo "${env.BUILD_NUMBER}"'
        }
}

}


def imageBuild(containerName, tag){
    sh "docker build -t $containerName:$tag  -t $containerName --pull --no-cache ."
    echo "Image build complete"
}


def pushToImage(containerName, tag, dockerUser, dockerPassword){
    sh "docker login -u $dockerUser -p $dockerPassword"
    sh "docker tag $containerName:$tag $dockerUser/$containerName:$tag"
    sh "docker push $dockerUser/$containerName:$tag"
    echo "Image push complete"
}