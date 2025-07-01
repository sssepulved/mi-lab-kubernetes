pipeline {
    agent any // Donde se ejecutará el pipeline

    tools {
        // Asegúrate de que las herramientas (ej. Docker) estén disponibles si no compartiste el socket.
        // Si compartiste el socket, Jenkins usará el Docker de tu host.
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/sssepulved/mi-lab-kubernetes.git' // Reemplaza con la URL de tu repo GitHub
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Construye la imagen Docker
                    sh 'docker build -t mi-app-web:latest ./app' // Siempre construimos la 'latest'
                    // Taggear con el ID del build de Jenkins para versiones específicas
                    sh "docker tag mi-app-web:latest mi-app-web:${env.BUILD_NUMBER}"
                }
            }
        }

        stage('Push Docker Image (Opcional - requiere Docker Hub)') {
            steps {
                script {
                    // Si tienes una cuenta en Docker Hub, puedes subir la imagen
                    // Necesitas configurar tus credenciales de Docker Hub en Jenkins
                    // sh 'docker login -u TU_USUARIO_DOCKERHUB -p TU_PASSWORD_DOCKERHUB'
                    // sh 'docker push TU_USUARIO_DOCKERHUB/mi-app-web:latest'
                    // sh "docker push TU_USUARIO_DOCKERHUB/mi-app-web:${env.BUILD_NUMBER}"
                    echo "Skipping Docker Hub push for local lab setup."
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Aplicar el Deployment y Service a Kubernetes
                    // Asegúrate de que kubectl esté configurado para apuntar a tu clúster de Docker Desktop
                    sh 'kubectl config use-context docker-desktop' // O 'minikube' si usas Minikube
                    sh 'kubectl apply -f ./kubernetes/deployment.yaml'
                    sh 'kubectl apply -f ./kubernetes/service.yaml'

                    // Opcional: Actualizar la imagen en el Deployment sin downtime
                    // Esto asume que tu Deployment ya existe
                    sh "kubectl set image deployment/mi-app-deployment mi-app-container=mi-app-web:latest"
                }
            }
        }
    }
}