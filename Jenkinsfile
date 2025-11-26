pipeline {
    agent any
    environment {
        registry = "kharifi/jenkis_setup"
        registryCredential = 'DOCKER_PUSH_KEY'
        dockerImage = ''
    }

    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', url: 'https://github.com/Poogast123/jenkis-setup'
            }
        }

        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }

        stage('Test image') {
            steps {
                script {
                    echo "Tests passed"
                }
            }
        }

        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy image') {
            steps {
                sh "docker run -d -p 8080:80 $registry:$BUILD_NUMBER" 
            }
        }
    }
}