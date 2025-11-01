pipeline {
    // Run the pipeline on the main Jenkins server/agent
    agent any 

    // Environment variables used throughout the pipeline
    environment {
        // Name for the Docker image we will build
        DOCKER_IMAGE = "car-pooling-api"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Jenkins automatically checks out the code here
                echo 'Code successfully checked out from GitHub.'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image: ${DOCKER_IMAGE}"
                    // This command uses the Dockerfile you created in Step 1
                    sh "docker build -t ${DOCKER_IMAGE}:latest ."
                }
            }
        }

        stage('Test Application') {
            steps {
                script {
                    echo "Starting containers for integration testing (API and MySQL)..."
                    // Spin up both containers as defined in docker-compose.yml
                    sh 'docker-compose up --build -d'
                    
                    // Wait for MySQL to initialize before testing
                    sh 'sleep 30' 
                    
                    // This is the CRITICAL line: You must replace 'npm test' 
                    // with the actual command to run your unit/integration tests
                    sh 'docker exec node-api <YOUR_TEST_COMMAND_HERE>' 
                    
                    echo "Tests completed. Stopping containers."
                    
                    // Stop and remove the containers after testing is done
                    sh 'docker-compose down'
                }
            }
        }
        
        stage('Deploy to VM') {
            steps {
                echo 'Deployment logic to push the image to your VM would be placed here.'
                echo 'The built image is currently stored locally on the Jenkins host.'
            }
        }
    }
}
