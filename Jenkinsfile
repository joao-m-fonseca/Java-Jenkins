pipeline {
    agent any
    
    parameters { 
        string(name: 'DOCKER_IMAGE_NAME', defaultValue: 'javapp', description: 'Adicionar um nome a imagem docker')
        string(name: 'DOCKER_CONTAINER_NAME', defaultValue: 'javapp', description: 'Adicionar um nome do container')
        string(name: 'DOCKER_CONTAINER_PORT', defaultValue: '8080', description: 'Adicionar o Port do container')
    }
    stages {
        stage ('CleanResources') {
            agent any
            steps
            {
                cleanWs()
            }
        }
        stage ('Maven Clean') {
            agent any
            tools {
                maven 'maven'
                jdk 'jdk11'
            }
            steps {
                sh './mvnw clean install'
            }
        }
        stage ('Build Docker Image') {
                agent any
                steps {
                        sh 'docker build -t "${DOCKER_IMAGE_NAME}" backend-java/backend-java/book/'
                }
            }
            stage ('Run Docker Container') {
                agent any
                steps {
                      sh """  docker ps -a \
                        | awk '{ print \$1,\$2 }' \
                        | grep "${DOCKER_IMAGE_NAME}" \
                        | awk '{print \$1 }' \
                        | xargs -I {} docker rm -f {}"""
                    sh 'docker run -d -p 8080:${DOCKER_CONTAINER_PORT} --name "${DOCKER_CONTAINER_NAME}" "${DOCKER_IMAGE_NAME}"'
                }
            }
        }
    }