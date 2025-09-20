pipeline {
    agent any

    tools {
        nodejs "node18"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/ngigi-nyoro/gallery.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                echo "No tests found. Skipping..."
            }
        }

        stage('Deploy to Render') {
            steps {
                withCredentials([string(credentialsId: 'RENDER_API_KEY', variable: 'RENDER_API_KEY')]) {
                    sh '''
                        echo "Triggering Render deploy..."
                        curl -X POST \
                          -H "Authorization: Bearer $RENDER_API_KEY" \
                          -H "Accept: application/json" \
                          -H "Content-Type: application/json" \
                          -d '{}' \
                          https://api.render.com/v1/services/srv-d37d89emcj7s73fimbvg/deploys
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline executed successfully and deployment triggered!"
        }
        failure {
            echo "❌ Pipeline failed."
        }
    }
}
