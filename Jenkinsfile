pipeline {
   agent none
   environment {
        ENV = "dev"
        NODE = "Build-server"
    }

   stages {
    stage('Build Image') {
        agent {
            node {
                label "Build-server"
                customWorkspace "/mnt/c/users/hungp/ubuntu/devops-training-$ENV/"
            }
        }
        environment {
            TAG = sh(returnStdout: true, script: "git rev-parse -short=10 HEAD | tail -n +2").trim()
        }
        steps {
            sh "docker build nodejs/. -t devops-training-nodejs-$ENV:latest --build-arg BUILD_ENV=$ENV -f nodejs/Dockerfile"

            sh "cat docker.txt | docker login -u hungpv1195 --password-stdin"
            // tag docker image
            sh "docker tag devops-training-nodejs-$ENV:latest hungpv1195/demo-build:$TAG"

            //push docker image to docker hub
            sh "docker push hungpv1195/demo-build:$TAG"

            // remove docker image to reduce space on build server	
            sh "docker rmi -f hungpv1195/demo-build:$TAG"
        } 
    }
	stage ("Deploy") {
	    agent {
            node {
                label "Target-Server"
                customWorkspace "D:/SETA/Workspace/Jenkins/devops-training-$ENV/"
            }
        }
        environment {
            TAG = sh(returnStdout: true, script: "git rev-parse -short=10 HEAD | tail -n +2").trim()
        }
	    steps {
            sh "sed -i 's/{tag}/$TAG/g' D:/SETA/Workspace/Jenkins/devops-training-$ENV/docker-compose.yaml"
            sh "docker compose up -d"
        }      
    }
   }
    
}
