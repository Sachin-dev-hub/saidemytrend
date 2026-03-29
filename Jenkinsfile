pipeline {
    agent any

    tools {
        maven 'M2'   // must match Global Tool Configuration
        jdk 'JAVA_HOME'      // must match Global Tool Configuration
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Sachin-dev-hub/my-irctc.git'
            }
        }

        stage('Build') {
            steps {
                echo '========== Build Started =========='
                sh 'mvn clean package -DskipTests'
                echo '========== Build Completed =========='
            }
        }

        stage('Unit Test Report') {
            steps {
                echo '========== Test Report =========='
                sh 'mvn surefire-report:report'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sachin-sonarqube-server') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

    }

    post {
        success {
            archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            echo '✅ Build & SonarQube analysis SUCCESS'
        }
        failure {
            echo '❌ Build FAILED'
        }
    }
}
