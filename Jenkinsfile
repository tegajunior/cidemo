pipeline {
    agent any

    stages {
        // Stage 1: Checkout code from GitHub using GitHub PAT
        stage('Checkout Code') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/tegajunior/cidemo.git',
                        credentialsId: 'github-cidemo' // Your GitHub credentials ID in Jenkins
                    ]]
                ])
            }
        }

        
        // Stage 2: Compile the application and resolve dependencies
        stage('Build with Maven') {
            steps {
                sh 'mvn clean compile'
            }
        }
        // Stage 3: Run Unit Tests
        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }

        // Stage 4: Package Application
        stage('Package Application') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        // Stage 5: Build Docker image
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t tegajunior/cidemo:v${BUILD_ID} .'
            }
        }

        // Stage 6: Push Docker image to Docker Hub
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB') {
                        sh 'docker push tegajunior/cidemo:v${BUILD_ID}'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
