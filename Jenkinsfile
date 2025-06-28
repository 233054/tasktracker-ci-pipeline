pipeline {
    agent any

    environment {
        NODE_ENV = 'test'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-github-username/tasktracker-ci-pipeline.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Lint Check') {
            steps {
                sh 'npx eslint .'
            }
        }

        stage('Code Coverage') {
            steps {
                sh 'npm run coverage' // or use jest --coverage
                junit '**/coverage/*.xml'
            }
        }

        stage('Archive Artifacts') {
            when {
                expression { return env.GIT_BRANCH == 'refs/tags/release' }
            }
            steps {
                archiveArtifacts artifacts: '**/build/**', fingerprint: true
            }
        }
    }

    post {
        failure {
            mail to: 'your_email@gmail.com',
                 subject: "❌ Jenkins Build Failed",
                 body: "Build #${env.BUILD_NUMBER} failed. Check console output at ${env.BUILD_URL}"
        }
        success {
            mail to: 'your_email@gmail.com',
                 subject: "✅ Jenkins Build Passed",
                 body: "Build #${env.BUILD_NUMBER} passed. Details: ${env.BUILD_URL}"
        }
    }
}
