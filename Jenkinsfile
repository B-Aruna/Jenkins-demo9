pipeline {
    agent any
    environment {
        // You can set environment variables here
        MAVEN_OPTS = "-Dmaven.test.failure.ignore=true"
    }
    tools {
        maven 'Maven 3'  // Define your Maven installation name from Jenkins Global Tool Configuration
    }   
    stages {
       stage('Build') {
            steps {
                echo 'Building the application...'
                sleep 5
            }
        }
       stage('Registering build artifact') {
            steps {
                echo 'Registering the metadata'
                echo 'Another echo to make the pipeline a bit more complex'
                registerBuildArtifactMetadata(
                    name: "Jenkins-demo9",
                    version: "1.0.0",
                    type: "docker",
                    url: "http://localhost:1111",
                    digest: "6u637064707039346163663237383930",
                    label: "prod"
                )
                sleep 5
            }
        }
        stage('Unit Test') {
            steps {
                sh 'mvn clean test'
            }
        }
        stage('Publish Test Results') {
            steps {
                junit 'target/surefire-reports/*.xml'
                sleep 5
            }
        } 
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                sleep 5
            }
        }
    }
}
