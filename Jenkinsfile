pipeline {
    agent { label 'dev-agent' }

    environment {
        IMAGE_NAME = "node-app"
    }

    stages {

        stage("Code Clone") {
            steps {
                echo "Code Clone Stage"
                git branch: "main", url: "https://github.com/vaibhavchudari/node-app.git"
                // ⚠️ Changed from "master" → "main"
            }
        }

        stage("Code Build") {
            steps {
                echo "Code Build Stage"
                sh "docker build -t $IMAGE_NAME ."
            }
        }

        stage("Push To DockerHub") {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: "dockerHubCreds",
                        usernameVariable: "DOCKER_USER",
                        passwordVariable: "DOCKER_PASS"
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag node-app:latest $DOCKER_USER/node-app:latest
                        docker push $DOCKER_USER/node-app:latest
                    '''
                }
            }
        }

        stage("Deploy"){
    steps{
        sh "docker-compose down"
        sh "docker-compose up -d --build"
    }
}
    }
}
