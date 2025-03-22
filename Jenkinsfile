pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checks out code from the specified Git repository
                git url: 'https://github.com/dayananda30/Jenkin.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                // Replace the echo with your actual build command(s)
                echo "Building the project..."
            }
        }
    }
}
