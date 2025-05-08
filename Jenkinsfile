pipeline {
    agent any

    environment {
        SONAR_URL = 'http://localhost:9000'
        SONAR_LOGIN = 'zeineb'
        SONAR_PASSWORD = '20076812'
        DOCKER_IMAGE = 'meriemraddadi/studentdashboard:0.0.1'
    }

    stages {
        stage('Checkout Git repository') {
            steps {
                echo '✅ Simulation: Pulling Git repository from GitHub'
                echo 'git clone https://github.com/meriemraddadi/StudentDashboard.git'
            }
        }

        stage('Maven Build') {
            steps {
                echo '✅ Simulation: Running mvn clean install'
                echo 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo '✅ Simulation: Running SonarQube analysis'
                echo """
                    mvn sonar:sonar \\
                    -Dsonar.projectKey=StudentDashboard \\
                    -Dsonar.host.url=$SONAR_URL \\
                    -Dsonar.login=$SONAR_LOGIN \\
                    -Dsonar.password=$SONAR_PASSWORD
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '✅ Simulation: Building Docker image'
                echo "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                echo '✅ Simulation: Logging into DockerHub and pushing image'
                echo "docker login -u meriemraddadi -p ******"
                echo "docker push $DOCKER_IMAGE"
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                echo '✅ Simulation: Deploying with Docker Compose'
                echo 'docker-compose up -d'
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline simulé exécuté avec succès !'
        }
        failure {
            echo '❌ Le pipeline simulé a échoué. (Ce serait très étrange ici)'
        }
    }
}
