pipeline {
    agent { label "vinod" }

    stages {

        stage("Cleanup") {
            steps {
                echo "Cleaning old Docker resources"
                sh "docker system prune -af || true"
            }
        }

        stage("Code") {
            steps {
                echo "Cloning code"
                git url: "https://github.com/LondheShubham153/django-notes-app", branch: "main"
            }
        }

        stage("Build") {
            steps {
                echo "Building Docker image"
                sh "docker build -t notes-app:latest ."
            }
        }

        stage("Test") {
            steps {
                echo "Running Trivy scan"
                sh """
                trivy image --scanners vuln --exit-code 1 notes-app || echo "Vulnerabilities found"
                """
            }
        }
        
        stage("Push") {
            steps {
                echo "This is pushing the image to Docker Hub"
                withCredentials([usernamePassword(
                    "credentialsId":"dockerHubCred",
                    passwordVariable:"dockerHubPass",
                    usernameVariable:"dockerHubUser")]){
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                        sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                        sh "docker push ${env.dockerHubUser}/notes-app:latest"
                    }
            }
        }

        stage("Deploy") {
            steps {
                echo "Deploying using docker-compose"

                sh "docker-compose down || true"
                sh "docker-compose up -d --build"
            }
        }
    }
}
