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
                sh 'npm run test'
            }
            post {
                failure {
                    mail to: 'nyorojnr@gmail.com',
                         subject: "Tests Failed: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                         body: """Hello,

                            The test stage failed in job: ${env.JOB_NAME}.
                            Build number: ${env.BUILD_NUMBER}
                            Check logs: ${env.BUILD_URL}

                            Regards,
                            Jenkins
                            """
                }
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
