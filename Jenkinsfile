pipeline {

    agent any

    tools { 
        maven 'my-maven' 
    }
 
    stages {

        stage('Build with Maven') {
            steps {
                sh 'ls -la'

                sh 'mvn --version'
                sh 'java -version'
                sh 'mvn clean package -Dmaven.test.failure.ignore=true'
            }
        }

      
        stage('Packaging/Pushing imagae') {

            steps {
                withDockerRegistry(credentialsId: 'Dockerhub', url: 'https://index.docker.io/v1/') {
                    sh 'docker build -t datdo27122003/springboot -f Dockerfile.build .'
                    sh 'docker push datdo27122003/springboot'
                }
            }
        }
        stage('Deploy container') {

            steps {
                withCredentials([file(credentialsId: 'main_key_pair', variable: 'main_key_pair')]) {
                    sh 'ls -la'
                    sh """
                    if [ -f "main_key_pair.pem" ]; then
                        echo "File exists: main_key_pair"
                    else
                        cp "$MAIN_KEY_PAIR" main_key_pair.pem
                        chmod 400 main_key_pair.pem
                    fi
                    """
                    sh 'ansible --version'
                    sh 'ls -la'
                    sh 'ansible-playbook -i inventory.ini --private-key main_key_pair.pem playbook.yml -vvvv'
                }
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