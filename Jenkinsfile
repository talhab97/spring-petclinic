pipeline {
    agent any

    tools {
        maven 'Maven 3'     // Set up in Jenkins → Global Tool Configuration
        jdk 'JDK 17'        // Set up in Jenkins → Global Tool Configuration
    }

    environment {
        IMAGE_NAME = 'petclinic'
        CONTAINER_NAME = 'petclinic-container'
    }

    stages {
        stage('Clone') {
            steps {
                git url: 'https://github.com/talhab97/spring-petclinic.git', branch: 'main'
            }
        }

        stage('Build JAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Stop Existing Container') {
            steps {
                sh '''
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8081:8080 --name $CONTAINER_NAME $IMAGE_NAME'
            }
        }
    }
}
