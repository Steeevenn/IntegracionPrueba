pipeline {
    agent any
    parameters {
        choice(name: 'PIPELINE_STAGE', choices: ['build', 'deploy'], description: 'Seleccione el pipeline a ejecutar')
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Steeevenn/Integracioncontinua.git'
            }
        }
        stage('Build Pipeline') {
            when {
                expression { params.PIPELINE_STAGE == 'build' }
            }
            stages {
                stage('Build Spring App') {
                    steps {
                        script {
                            docker.build('spring-app2', './backend/crud-application')
                        }
                    }
                }
                stage('Test Spring App') {
                    steps {
                        sh 'docker run --rm spring-app2 ./mvnw test'
                    }
                }
                stage('Build Frontend') {
                    steps {
                        script {
                            docker.build('frontend', './frontend/crudfront')
                        }
                    }
                }
                stage('Test Frontend') {
                    steps {
                        sh 'docker run --rm frontend npm test'
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
                    sh 'docker-compose down'
                    sh 'docker-compose up -d'
                }
            }
        }
    }
}
