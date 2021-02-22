pipeline {
    agent any
    parameters { 
        string(name: 'DOCKER_IMAGE_NAME', defaultValue: 'javapp', description: 'Adicionar um nome a imagem docker')
        string(name: 'DOCKER_CONTAINER_NAME', defaultValue: 'javapp', description: 'Adicionar um nome do container')
        string(name: 'DOCKER_CONTAINER_PORT', defaultValue: '3000', description: 'Adicionar o Port do container')
    }
    tools {
    //jdk 'jdk'
    maven 'maven'
    }
    stages {
        stage ('Maven Clean') {
            agent any
            steps {
               // sh 'chmod +x mvnw'
               // sh './mvnw clean install'
               sh ' maven clean install'
            }
        }
        stage ('Build Docker Image') {
            agent any
            steps {
                sh 'docker build -t "${DOCKER_IMAGE_NAME}" .'
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
                    sh 'docker run -d -p ${DOCKER_CONTAINER_PORT}:8080 --name "${DOCKER_CONTAINER_NAME}" "${DOCKER_IMAGE_NAME}"'
                }
            }
        stage ('CleanResources') {
            agent any
            steps
            {
                cleanWs()
            }
        }
        }
    }