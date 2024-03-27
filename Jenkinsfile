pipeline {
    agent any // Specifies that the pipeline can run on any available agent

    stages {
        stage("Clone Code") {
            steps {
                echo 'Cloning the code'
                git url: 'https://github.com/hassanhah/django-notes-app.git', branch: 'main'
            }
        }
        
        stage("Build") {
            steps {
                echo 'Building the image'
                sh 'docker build -t syedhassan1/notes-app .'
                // Add steps to build your application
                // For example:
                // sh 'mvn clean install'
            }
        }
        
        stage("Push to Docker Hub") {
            steps {
                echo 'Pushing the built image to Docker Hub'
                withCredentials([usernamePassword(credentialsId:"dockerhub", usernameVariable:"DOCKERHUB_USER", passwordVariable:"DOCKERHUB_PASS")]) {
                    sh "docker login -u ${env.DOCKERHUB_USER} -p ${env.DOCKERHUB_PASS}"
                    
                }
                sh 'docker push syedhassan1/notes-app:latest'
                // Add steps to build Docker image and push it to Docker Hub
                // For example:
                // sh 'docker build -t your-image-name .'
                // sh 'docker push your-image-name'
            }
        }
        
        stage("Deploy") {
            steps {
                  echo 'Deploying the container using docker-compose'
                  sh 'docker-compose down || true' // Stop and remove existing containers, ignore errors if there are none
                  sh 'docker-compose up -d' // Start containers defined in the docker-compose.yml file in detached mode
        }
        }
    }
}
