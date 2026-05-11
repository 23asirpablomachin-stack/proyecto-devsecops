pipeline {
    agent any
    stages {
        stage('Descargar Código') {
            steps {
                echo 'Clonando el repositorio...'
                git branch: 'desarrollo', url: 'https://github.com/23asirpablomachin-stack/proyecto-devsecops.git'
            }
        }
        stage('Construir Imagen (Build)') {
            steps {
                echo 'Construyendo el contenedor...'
                sh 'docker build -t mi-app-segura:latest .'
            }
        }
        stage('Análisis de Seguridad (Trivy)') {
            steps {
                echo 'Buscando vulnerabilidades CRÍTICAS...'
                // Este comando hace que el pipeline falle si hay vulnerabilidades críticas [cite: 22, 23]
                sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasecurity/trivy:latest image --exit-code 1 --severity CRITICAL mi-app-segura:latest'
            }
        }
        stage('Despliegue en Producción (CD)') {
            steps {
                echo '¡Imagen limpia! Desplegando en el servidor...'
                sh 'docker stop app-produccion || true'
                sh 'docker rm app-produccion || true'
                sh 'docker run -d --name app-produccion mi-app-segura:latest'
            }
        }
    }
}
