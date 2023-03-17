node {
    stage("Git Clone"){

        git credentialsId: 'Git-Hub-Password', url: 'https://github.com/ManjunathGR607/Myresume.git'
        
    stage("Docker build"){
        sh 'docker version'
        sh 'docker rmi manjunathgr/resume:latest'
        sh 'docker build --no-cache -t resume:latest .'
        sh 'docker tag resume manjunathgr/resume:$BUILD_NUMBER'
        sh 'docker tag resume manjunathgr/resume:latest'
    }

    withCredentials([string(credentialsId: 'Docker-Hub-Password', variable: 'PASSWORD')]) {
        sh 'docker login -u manjunathgr -p $PASSWORD'
    }

    stage("Push Image to Docker Hub"){
        sh 'docker push  manjunathgr/resume:$BUILD_NUMBER'
        sh 'docker push  manjunathgr/resume:latest'
    }
    
    stage("kubernetes deployment"){
        sh 'sed -i "s/_BUILD_NUMBER_/${BUILD_NUMBER}/" deploymentservice.yaml'
        sh 'kubectl apply -f deploymentservice.yaml'
        }
    }
}
