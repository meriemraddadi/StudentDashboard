pipeline {
    agent any

    tools {
        maven 'M2_HOME'
    }

    stages {
        stage('Checkout Git repository') {
            steps {
                echo 'Pulling Git repository'
                git branch: 'master', url: 'https://github.com/meriemraddadi/studentDashboard.git'
            }
        }

        stage('Maven Clean Compile') {
            steps {
                echo 'Running Maven Clean and Compile'
                sh 'mvn clean compile'
            }
        }

        stage('Maven Install') {
            steps {
                echo 'Running Maven Install'
                sh 'mvn install'
            }
        }

        stage('Build Package') {
            steps {
                echo 'Running Maven Package'
                sh 'mvn package'
            }
        }

       stage('SonarQube Analysis') {
           steps {
               sh '''
                   mvn sonar:sonar \
                       -Dsonar.host.url=http://192.168.33.10:9000 \
                       -Dsonar.login=admin \
                       -Dsonar.password=201jMt2340@@ \
    
           }
       }



        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker Image'
                    def dockerImage = docker.build("meriemraddadi/kaddem:0.0.1")
                }
            }
        }

        stage('Deploy Image to DockerHub') {
            steps {
                script {
                    echo 'Logging into DockerHub and Pushing Image'
                    sh 'docker login -u meriemraddadi -p 20076812'
                    sh 'docker push meriemraddadi/StudentDashboard:0.0.1'
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                script {
                    echo 'Deploying with Docker Compose'
                    sh 'docker-compose up -d'
                }
            }
        }
}
