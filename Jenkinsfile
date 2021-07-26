pipeline {
    agent any
    
    tools {
        maven 'M3'
    }
    
    stages {
        stage('Cleaning workspace') {
            steps {
                sh 'echo "---=--- Cleaning Stage ---=---"'
                git branch: 'main', url:'https://github.com/BertRomain/SimpleBoot.git'
                sh 'mvn clean'
                script {
                try {
                sh 'docker stop myboot && docker rm myboot'
                } catch (Exception e) {
                    sh 'echo "---=--- No container to remove ---=---" '
                }
                }
            }
        }
        
        stage('Checkout') {
            steps {
                sh 'echo "---=--- Checkout ---=---"'
            }
        }
        
        stage('Compile') {
            steps {
                sh 'echo "---=--- Compile ---=---"'
                sh 'mvn clean compile'
            }
        }
        
        stage('Test') {
            steps {
                sh 'echo "---=--- Test ---=---"'
                sh 'mvn test'
            }
        }
        
        stage('Package') {
            steps {
                sh 'echo "---=--- Package ---=---"'
                sh 'mvn -DskipTests package'
            }
        }
        
        stage('Docker') {
            steps {
                sh 'echo "---=--- Docker ---=---"'
              sh 'docker build -t bertromain/myboot:1.0 .'

            }
        }
        
        stage('Deploy to AWS') {
            steps {
                sh 'echo "---=--- Deploy to local ---=---"'
                sh'docker run -d --name myboot -p 8180:8180 bertromain/myboot:1.0'
            }
        }
    }
}
