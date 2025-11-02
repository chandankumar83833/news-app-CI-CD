pipeline {
    agent any

    tools {
        jdk 'jdk17'          // Make sure you have this configured in Jenkins
        maven 'maven3'       // Same for Maven (Manage Jenkins → Global Tool Configuration)
    }

    environment {
        APP_NAME = 'news-app'
    }

    triggers {
        // Automatically builds when you push to GitHub (requires webhook)
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning source code from GitHub...'
                git branch: 'main', url: 'https://github.com/chandankumar83833/news-app-CI-CD.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project with Maven...'
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test'
            }
            post {
                always {
                    // Publishes test results to Jenkins UI
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the application...'
                sh 'mvn package -DskipTests'
            }
        }

        stage('Archive') {
            steps {
                echo 'Archiving build artifacts...'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "✅ Build succeeded for ${APP_NAME}"
        }
        failure {
            echo "❌ Build failed for ${APP_NAME}"
        }
    }
}
