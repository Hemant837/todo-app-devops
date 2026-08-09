pipeline {
    agent any

    environment {
    FRONTEND_IMAGE = "todo-frontend:jenkins"
    BACKEND_IMAGE = "todo-backend:jenkins"
    PORT = "5000"
    MONGO_URI = "mongodb://mongo:27017/taskdb"
    }

    stages {
        stage("Checkout Code") {
            steps {
                git url: "https://github.com/Hemant837/todo-app-devops.git", branch: "main"
            }
        }
        stage("Prepare Environment") {
            steps {
                sh '''
                    mkdir -p backend
                    cat > backend/.env << EOF
                PORT=$PORT
                MONGO_URI=$MONGO_URI
                EOF
                '''
            }
        }
        stage("Build Docker Images") {
            steps {
                sh '''
                    echo "Building backend image..."
                    docker build -t $BACKEND_IMAGE ./backend

                    
                    echo "Building frontend image..."
                    docker build -t $FRONTEND_IMAGE ./frontend --build-arg VITE_API_URL=http://localhost:5000/api
                '''
            }
        }
        stage("Run Containers with docker compose") {
            steps {
                sh '''
                    echo "Running containers with docker compose..."
                    docker compose up -d

                    echo "showing running containers..."
                    docker ps

                    echo "===== Backend Logs ====="
                    docker logs backend || true

                    echo "===== Frontend Logs ====="
                    docker logs frontend || true

                '''
            }
        }
    }
}