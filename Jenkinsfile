pipeline {
    agent any
    tools {
        maven 'Maven-3.9'   // Doit correspondre au nom donné à l'Étape 2
        jdk 'JDK-21'        // Doit correspondre au nom donné à l'Étape 2
    }
    triggers {
        githubPush()        // Active le déclenchement par webhook
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Tests Unitaires') {
            steps {
                dir('backend') {
                    sh 'mvn -B clean test'
                }
            }
            post {
                always {
                    junit 'backend/target/surefire-reports/*.xml'
                }
            }
        }
        stage('Build & Package') {
            steps {
                dir('backend') {
                    sh 'mvn -B package -DskipTests'
                }
            }
            post {
                success {
                    archiveArtifacts artifacts: 'backend/target/*.jar, backend/target/*.war', fingerprint: true
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}