def CONTAINER_NAME="getintodevops-hellonode"
def CONTAINER_TAG="${env.BUILD_NUMBER}"
//def USERNAME="khzied"

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


    stage('Push to Docker Registry'){
        withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            pushToImage(CONTAINER_NAME, CONTAINER_TAG, USERNAME, PASSWORD)
        }
}

}


def imageBuild(containerName, tag){
    //sh "docker build -t $containerName:$tag  -t $containerName --pull --no-cache ."
    sh "docker build -t khzied/$containerName:$tag  -t $containerName --pull --no-cache ."
    echo "Image build complete"
}


def pushToImage(containerName, tag, dockerUser, dockerPassword){
    sh "docker login -u $dockerUser -p $dockerPassword"
//    sh "docker tag $containerName:$tag $dockerUser/$containerName:$tag"
    sh "docker push $dockerUser/$containerName:$tag"
    echo "Image push complete"
}