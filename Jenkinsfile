pipeline{

    agent any

    environment {

        DOCKER_IMAGE = "bhagyashri45/nginx-webapp"
        DOCKER_TAG = 'latest'
        
    }

    stages{

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'Jenkins-Sonarqube-Token', variable: 'SONAR_TOKEN')]) {
                    sh """
                       sonar-scanner \
                          -Dsonar.projectKey=Nginx-webapp \
                          -Dsonar.projectName=Nginx-webapp \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=http://localhost:9000 \
                          -Dsonar.login=$SONAR_TOKEN
                          
                    """
                }
            }
        }

        stage('Docker Build') {

            steps {
                sh 'echo "Building Docker Image ..."'
                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'

            }

        }

        stage('Push to DockerHub') {

            steps {

                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {

                    sh '''
                        echo "Docker logging in ..."
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        echo "Docker image push ..."
                        docker push $DOCKER_IMAGE:$DOCKER_TAG

                    '''

                }

            }

        } 



    }
    post {

        always {

            cleanWs()

        }

    }
}
