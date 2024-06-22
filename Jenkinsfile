pipeline {
    agent any

    parameters {
        choice(name: 'PIPELINE_STAGE', choices: ['build', 'deploy'], description: 'Seleccione el pipeline a ejecutar')
    }

    stages {
        stage('Checkout') {
            steps {
                // Clonar el repositorio desde GitHub
                git branch: 'main', url: 'https://github.com/Steeevenn/Integracioncontinua.git'
            }
        }

        stage('Build Pipeline') {
            when {
                expression { params.PIPELINE_STAGE == 'build' }
            }
            stages {
                stage('Build app'){
                    steps{
                            dir('backend/crud-application'){
                            sh 'mvn clean package'
                        }
                    }
                }
                stage('Build Spring App') {
                    steps {
                        script {
                            // Construir la imagen Docker para la aplicación Spring Boot
                            docker.build('spring-app2', './backend/crud-application')
                        }
                    }
                }



                stage('Build Frontend') {
                    steps {
                        script {
                            // Construir la imagen Docker para el frontend
                            docker.build('frontend', './frontend/crudfront')
                        }
                    }
                }

          
            }
        }

        stage('Deploy Pipeline') {
            when {
                expression { params.PIPELINE_STAGE == 'deploy' }
            }
            steps {
                script {
                    // Detener y eliminar contenedores existentes antes de desplegar
                    sh 'docker-compose down'
                    
                    // Levantar contenedores definidos en docker-compose.yml
                    sh 'docker-compose up -d'
                }
            }
        }
    }
}
