pipeline {
    tools {
        jdk 'myjava'
        maven 'mymaven'
    }
    
    agent any
    
    stages {
        stage('Checkout ') {
            agent {
                label 'master'
            }
            steps {
                echo 'Cloning...'
                git 'https://github.com/bensonezea/samplejenkinsproject.git'
            }
        }
        
        stage('Compile on slave2') {
            agent {
                label 'slave1'
            }
            steps {
                echo 'Compiling...'
                sh 'mvn compile'
            }
        }
        
        stage('CodeReview on slave1') {
            agent {
                label 'slave2'
            }
            steps {
                echo 'Code Review...'
                sh 'mvn pmd:pmd'
            }
        }
        
        stage('UnitTest on slave2') {
            agent {
                label 'slave2'
            }
            steps {
                echo 'Testing...'
                sh 'mvn test'
            }
            post {
                success {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Package on master') {
            agent {
                label 'master'
            }
            steps {
                echo 'Packaging...'
                sh 'mvn package'
            }
        }
    }
}

