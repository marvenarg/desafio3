pipeline {
    agent any

    environment {
        IMAGE_NAME = 'mysven/desafio3'
        IMAGE_TAG = "v${BUILD_NUMBER}"
        CONTAINER_NAME = 'desafio3_container'
        APP_PORT = '5000'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials' // ID definido en Jenkins
    }

    // === TRIGGERS (activar el que sea necesario) ===
    //triggers {
        // githubPush()        // 🔗 Se ejecuta con cada push a GitHub (requiere webhook)
        // cron('H/15 * * * *') // ⏰ Se ejecuta cada 15 minutos (comentá si no lo usás)
    //}

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/marvenarg/desafio2.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -f webapp/Dockerfile -t ${IMAGE_NAME}:${IMAGE_TAG} webapp/
                    docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${IMAGE_NAME}:latest
                """
            }
        }

        stage('Run Container (test)') {
            steps {
                script {
                    sh """
                        if [ \$(docker ps -a -q -f name=${CONTAINER_NAME}) ]; then
                            docker stop ${CONTAINER_NAME} || true
                            docker rm ${CONTAINER_NAME} || true
                        fi
                        docker run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:${APP_PORT} ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }

        stage('Health Check') {
            steps {
                script {
                    sleep 5
                    sh "docker exec ${CONTAINER_NAME} test -f /var/www/html/index.php"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKER_CREDENTIALS_ID}",
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh """
                        echo \$PASS | docker login -u \$USER --password-stdin
                        docker push ${IMAGE_NAME}:${IMAGE_TAG}
                        docker push ${IMAGE_NAME}:latest
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh "docker rm -f ${CONTAINER_NAME} || true"
            sh "docker logout"
        }
        failure {
            echo '❌ El Pipeline ha fallado. Revisión necesaria.'
        }
        success {
            echo '✅ Pipeline ejecutado con éxito.'
        }
    }
}
