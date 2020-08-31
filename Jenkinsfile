node{
    stage("Git Clone"){
        git credentialsId: 'GITHUBID', url: 'https://github.com/nagenra87k/jenkins-kubernetes-pipeline.git'
    }
    stage("MVN Clean Build"){
        def mavenHome = tool name:"Maven-3.6.1", type: "maven"
        def mavenCMD = "${mavenHome}/bin/mvn"
        sh "${mavenCMD} clean package"
    }
    stage("Build Docker Images"){
        sh "docker build -t nagendra87k/jenkins-kubernetes-pipeline ."
    }
    stage("Push Docker Images"){
        withCredentials([usernameColonPassword(credentialsId: 'DOCKER_HUB_CREDENCIAL', variable: 'DOCKER_HUB_CREDENCIAL')]) {
            sh "docker login -u nagendra87k -p ${DOCKER_HUB_CREDENCIAL}"
        }
        sh "docker push nagendra87k/hello-world-rest-api:0.0.4-SNAPSHOT"
    }
    stage('Apply Kubernetes files') {
    withKubeConfig([credentialsId: 'KUBERNETES', serverUrl: 'https://35.224.15.130']) {
      sh 'kubectl apply -f deployment.yaml'
    }
    }
    
   
}
