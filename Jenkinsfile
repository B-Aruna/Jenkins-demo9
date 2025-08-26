pipeline {
    agent any
    environment {
        MAVEN_OPTS = "-Dmaven.test.failure.ignore=true"
    }
    tools {
        maven 'Maven 3'
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
                    name: "jenkins-demo9",
                    version: "4.0.0",
                    type: "docker",
                    url: "http://localhost:1111",
                    digest: "6u637064707039346163663930",
                    label: "prod"
                )
                sleep 5
            }
        }
        stage('Unit Test') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    sh 'mvn clean test'
                }
            }
        }
        stage('Publish Test Results') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    junit 'target/surefire-reports/*.xml'
                }
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
    post {
        always {
            script {
                // Override any UNSTABLE/FAILURE status at the end
                currentBuild.result = 'SUCCESS'
            }
        }
    }
}
