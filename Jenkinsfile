pipeline{

    any agent

    environment {

        DOCKER_IMAGE = "bhagyashri45/nginx-webapp"
        DOCKER_TAG = 'latest'
        DOCKER_CREDENTIALS_ID = 'docker-hub-creds'

    }

    stages{


        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Bhgayashri-45/Nginx-webapp.git'
            }

        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'Jenkins-Sonarqube-Token', variable: 'SONAR_TOKEN')]) {
                    sh """
                        echo "sonar.sources=." >> sonar-project.properties"
                          -Dsonar.projectKey=Nginx-webapp \
                          -Dsonar.projectName=Nginx-webapp \
                          -Dsonar.host.url=http://localhost:9000 \
                          -X
                    """
                }
            }
        }

        stage('Docker Build') {

            steps {

                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'

            }

        }

        stage('Push to DockerHub') {

            steps {

                withCredentials([usernamePassword(credentialsId: '$DOCKER_CREDENTIALS_ID', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {

                    sh '''

                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin

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
