pipeline {
    agent any

    tools {
        // These must match names in Jenkins > Manage Jenkins > Global Tool Configuration
        jdk 'jdk17'          // or 'jdk11', depending on what you installed
        maven 'maven3'       // name of your Maven installation in Jenkins
    }

    triggers {
        // Automatically trigger build when pushing to GitHub (optional)
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
                echo 'Building Maven project...'
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
                    echo 'Publishing test results...'
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
