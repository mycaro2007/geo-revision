pipeline {
    agent any
    
    tools {
        maven 'M2_HOME'
    }

    stages {
        stage('SonarQube Scan') {
                    agent {docker { image 'maven:3-amazoncorretto-17-debian' }}
            steps {
                withSonarQubeEnv('sonarserver') {
                    sh 'mvn verify sonar:sonar -Dsonar.projectKey=mycaro2007_geo-revision'
                }
            }
        }

        stage('All Maven Commands') {
            steps {
                sh 'mvn clean test compile install package'
            }  
        }

        stage('Build Image and Push to ECR') {
            steps {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 001887083797.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker build -t geoapp .'
                sh 'docker tag geoapp:latest 001887083797.dkr.ecr.us-east-1.amazonaws.com/geoapp:latest'
                sh 'docker push 001887083797.dkr.ecr.us-east-1.amazonaws.com/geoapp:latest'
            }
        }
    }
}
