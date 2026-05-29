pipeline {

    agent { label 'vinod' }

    parameters {

        choice(
            name: 'BUILD_ENV',
            choices: ['dev', 'test', 'prod'],
            description: 'Select deployment environment'
        )

        booleanParam(
            name: 'DOCKER_PUSH',
            defaultValue: true,
            description: 'Push image to DockerHub'
        )

        string(
            name: 'IMAGE_TAG',
            defaultValue: 'latest',
            description: 'Docker image tag'
        )
    }

    environment {

        APP_NAME = "notes-app"

    }

    stages {

        stage('Code Clone') {
            steps {

                echo "Cloning code from GitHub"

                git branch: 'test',
                url: 'https://github.com/Deepali-Gobare/django-notes-app.git'

            }
        }

        stage('Build Docker Image') {
            steps {

                echo "Building Docker image"

                sh "docker build -t ${APP_NAME}:${IMAGE_TAG} ."

            }
        }

        stage('Push Docker Image') {

            when {
                expression { params.DOCKER_PUSH == true }
            }

            steps {

                echo "Pushing image to DockerHub"

                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubCred',
                    usernameVariable: 'dockerHubUser',
                    passwordVariable: 'dockerHubPass'
                )]) {

                    sh "docker login -u ${dockerHubUser} -p ${dockerHubPass}"

                    sh "docker tag ${APP_NAME}:${IMAGE_TAG} ${dockerHubUser}/${APP_NAME}:${IMAGE_TAG}"

                    sh "docker push ${dockerHubUser}/${APP_NAME}:${IMAGE_TAG}"

                }
            }
        }

        stage('Deploy') {
            steps {

                echo "Deploying application"

                echo "Selected Environment: ${BUILD_ENV}"

            }
        }
    }

    post {

        success {
            echo "Pipeline executed successfully"
        }

        failure {
            echo "Pipeline failed"
        }

        always {
            echo "Pipeline completed"
        }
    }
}