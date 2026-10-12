pipeline {
    agent { label 'agentnikhil' }

    environment {
        IMAGE_NAME = 'my-note-app'
    }

    stages {
        stage('Clone Code') {
            steps {
                echo 'Cloning the code'
                git url: 'https://github.com/suryacs999/django-notes-app.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the image'
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing the image to Docker Hub'
                withCredentials([usernamePassword(
                    credentialsId: 'dockercred',
                    usernameVariable: 'dockerHubUser',
                    passwordVariable: 'dockerHubPass'
                )]) {
                    sh '''
                        echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
                        docker tag ${IMAGE_NAME}:latest $dockerHubUser/${IMAGE_NAME}:latest
                        docker push $dockerHubUser/${IMAGE_NAME}:latest
                        docker logout
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the container'
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
            }
        }
    }
}
