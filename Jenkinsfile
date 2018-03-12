node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "d-pipe"
    registryHost = "127.0.0.1:30400/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName

    stage "Build"
    
        sh "docker build -t ${imageName} -f applications/d-pipe/Dockerfile applications/d-pipe"
    
    stage "Push"

        sh "docker push ${imageName}"

    stage "Deploy"

        sh "sed 's#127.0.0.1:30400/d-pipe:latest#'$BUILDIMG'#' applications/d-pipe/k8s/deployment.yaml | kubectl apply -f -"
        sh "kubectl rollout status deployment/d-pipe"

}
