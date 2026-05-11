pipeline {
    agent any
    stages {
        stage('Descargar Código') {
            steps {
                echo 'Clonando el repositorio...'
                // Descarga la rama de desarrollo [cite: 33]
                git branch: 'desarrollo', url: 'https://github.com/23asirpablomachin-stack/proyecto-devsecops.git'
            }
        }
        stage('Construir Imagen (Build)') {
            steps {
                echo 'Construyendo el contenedor...'
                // Crea la imagen localmente [cite: 39]
                sh 'docker build -t mi-app-segura:latest .'
            }
        }
        stage('Análisis de Seguridad (Trivy)') {
            steps {
                echo 'Buscando vulnerabilidades CRÍTICAS...'
                // El parámetro --exit-code 1 hará que el pipeline falle si hay vulnerabilidades [cite: 21, 22]
                sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasecurity/trivy:latest image --exit-code 1 --severity CRITICAL mi-app-segura:latest'
            }
        }
        stage('Despliegue en Producción (CD)') {
            steps {
                echo '¡Imagen limpia! Desplegando en el servidor...'
                // Detiene contenedores previos y arranca el nuevo [cite: 54, 57]
                sh 'docker stop app-produccion || true'
                sh 'docker rm app-produccion || true'
                sh 'docker run -d --name app-produccion mi-app-segura:latest'
            }
        }
    }
}
