pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Descargando código fuente desde el repositorio Git...'
                git branch: 'main', url: 'https://github.com/geralde154-ops/practica_workpress_docker.git'
            }
        }

        stage('Detener ambiente anterior') {
            steps {
                echo 'Deteniendo contenedores previos (si existen)...'
                sh 'docker compose down || true'
            }
        }

        stage('Desplegar ambiente') {
            steps {
                echo 'Levantando el ambiente con Docker Compose...'
                sh 'docker compose up -d'
            }
        }

        stage('Verificar despliegue') {
            steps {
                echo 'Verificando contenedores en ejecución...'
                sh 'docker ps'
                sh 'docker network ls'
                sh 'docker volume ls'
            }
        }
    }
}