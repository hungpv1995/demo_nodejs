pipeline {
   agent none
   environment {
        ENV = "${buildENV}"
        NODE = "Build-server"
        DB_PORT = "${ ENV == 'dev' ? '3306' : '3307' }"
        APP_PORT = "${ ENV == 'dev' ? '3000' : '3001' }"
        DOCKER_HUB_CRED = credentials('dockerhub')
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
            sh "docker build nodejs/. -t devops-training-nodejs-$ENV:latest --build-arg BUILD_ENV=$ENV --build-arg APP_PORT=$APP_PORT -f nodejs/Dockerfile"

            sh 'echo $DOCKER_HUB_CRED_PSW | docker login -u $DOCKER_HUB_CRED_USR --password-stdin'
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
