@Library("Shared") _
pipeline{
    agent { label "vinod" }
    stages{
        stage("Hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        stage("code"){
            steps{
                script{
                    clone("https://github.com/Divyjain0212/django-notes-app.git","main")
                }
            }
        }
        stage("Build"){
            steps{
                script{
                    docker_build("notes-app","latest","divyj0212")
                }
            }
        }
        stage("Test"){
            steps{
                echo "This is testing the code."
            }
        }
        stage("Push to docker hub"){
            steps{
                script{
                    docker_push("notes-app","latest","divyj0212")
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "This is deploying the code."
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}
