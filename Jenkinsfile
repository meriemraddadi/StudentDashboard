pipeline {
    agent any

    tools {
        maven 'M2_HOME' // Assure-toi que M2_HOME est bien défini dans Jenkins
    }

    environment {
        SONAR_URL = "http://192.168.33.10:9000"
        SONAR_LOGIN = "admin"
        SONAR_PASSWORD = "201jMt2340@@"
        DOCKER_IMAGE = "meriemraddadi/studentdashboard:0.0.1"
    }

    stages {
        stage('Checkout Git repository') {
            steps {
                echo 'Pulling Git repository'
                git branch: 'master', url: 'https://github.com/meriemraddadi/StudentDashboard.git'
            }
        }

        stage('Maven Build') {
            steps {
                echo 'Running Maven Clean Install'
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') { // Ce nom doit correspondre à la config dans Jenkins
                    sh """
                        mvn sonar:sonar \
                        -Dsonar.projectKey=StudentDashboard \
                        -Dsonar.host.url=$SONAR_URL \
                        -Dsonar.login=$SONAR_LOGIN \
                        -Dsonar.password=$SONAR_PASSWORD
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image'
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                echo 'Deploying with Docker Compose'
                sh 'docker-compose up -d'
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline exécuté avec succès !'
        }
        failure {
            echo '❌ Le pipeline a échoué.'
        }
    }
}
