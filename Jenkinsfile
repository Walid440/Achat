pipeline {
    agent any
    
    stages {
        stage('Pulling from git...'){
            steps{
               git branch : 'main',
                url : 'https://github.com/Walid/Achat'
            }
        }
        
        stage('Testing maven...'){
            steps {
                sh """mvn -versinon"""
            }
        }