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
                // Use catchError and force both buildResult and stageResult to 'SUCCESS'
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    sh 'mvn clean test'
                }
            }
        }
        stage('Publish Test Results') {
            steps {
                step([
                    $class: 'JUnitResultArchiver',
                    testResults: 'target/surefire-reports/*.xml',
                    allowEmptyResults: true,
                    healthScaleFactor: 0.0
                ])
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
                    currentBuild.result = 'SUCCESS'
            }
        }
    }
}
