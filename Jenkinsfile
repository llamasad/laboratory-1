pipeline {

    agent any

    tools { 
        maven 'my-maven' 
    }
 
    stages {

        stage('Build with Maven') {
            steps {
                sh 'ls -l'

                sh 'mvn --version'
                sh 'java -version'
                sh 'mvn clean package -Dmaven.test.failure.ignore=true'
            }
        }

      
        stage('Packaging/Pushing imagae') {

            steps {
                withDockerRegistry(credentialsId: 'Dockerhub', url: 'https://index.docker.io/v1/') {
                    sh 'docker build -t datdo27122003/springboot .'
                    sh 'docker push datdo27122003/springboot'
                }
            }
        }
        stage('Deploy Spring Boot to DEV') {
            steps {
                echo 'Deploying and cleaning'
                sh 'docker image pull datdo27122003/springboot'
                sh 'docker container stop springboot-container || echo "this container does not exist" '
                sh 'docker network create dev || echo "this network exists"'
                sh 'echo y | docker container prune '

                sh 'docker container run -d --rm --name springboot-container -p 8081:8080 --network dev datdo27122003/springboot'
            }
        }
 
    }
    post {
        // Clean after build
        always {
            cleanWs()
        }
    }
}