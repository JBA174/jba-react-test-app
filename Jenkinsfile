pipeline {
    agent any

    environment {
        // AWS_ACCOUNT_ID = 'your-account-id'
        // AWS_REGION = 'your-region'
        // ECR_REPO = 'your-repo-name'
        // IMAGE_TAG = 'latest'
        BUILD_DIR = 'build'  // Change to 'dist' if needed
    }

    stages {
        stage('Checkout') {
            steps {
                echo '👀 Checking out code...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo '🛠️ Installing dependencies...'
                    sh 'npm install'
                }
            }
        }

        stage('Build React App') {
            steps {
                script {
                    echo '🚧 Building React App...'
                    sh 'npm run build' // Generates the build in $BUILD_DIR
                }
            }
        }

        stage('Prepare Docker Context') {
            steps {
                script {
                    echo '💪 Prepare Docker Context...'
                    sh 'mkdir -p docker-build && cp -r $BUILD_DIR docker-build/' // Move build folder
                    sh 'cp Dockerfile docker-build/' // Copy Dockerfile
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo '🏗️ Build Docker Image...'
                    // sh '''
                    // cd docker-build
                    // docker build -t $ECR_REPO:$IMAGE_TAG .
                    // '''
                }
            }
        }

        stage('Login to ECR') {
            steps {
                script {
                    echo '🔐 Login to ECR...'
                    // sh 'aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com'
                }
            }
        }

        stage('Tag & Push Image') {
            steps {
                script {
                    echo '🚚 Tag & Push Image...'
                    // sh '''
                    // docker tag $ECR_REPO:$IMAGE_TAG $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$IMAGE_TAG
                    // docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$IMAGE_TAG
                    // '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build and deployment successful!'
        }
        failure {
            echo '❌ Build failed.'
        }
    }
}
