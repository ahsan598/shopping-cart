pipeline {
    agent any
    tools {
        jdk 'jdk17'    // Ensure this matches the JDK version installed in Jenkins
        maven 'maven3' // Ensure this matches the Maven version installed in Jenkins
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'  // Ensure 'sonar-scanner' matches the SonarScanner tool name in Jenkins
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ahsan598/jenkins-project1.git'
            }
        }


        stage('Code Compile') {
            steps {
                sh "mvn clean compile"
            }
        }


        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh 'mvn sonar:sonar'
                }
            }
        }


        stage('Code Build') {
            steps {
                sh "mvn clean install"
            }
        }



        stage('Deploy to Nexus') {
            steps {
                script {
                    sh "mvn deploy"
                }
            }
        }
    }


    post {
        always {
            cleanWs()  // Clean up workspace after build
        }
    }
}