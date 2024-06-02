pipeline {
    agent any

    environment {
        MAVEN_IMAGE = 'maven:3.8.6' // Updated to a valid Docker image
        SETTINGS_PATH = '/root/.m2/settings.xml'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git url: 'https://github.com/chithalrahul/mrdevops_nexus_helm_cicd_app.git', branch: 'main'
            }
        }

        stage('Sonar Analysis Status') {
            steps {
                script {
                    docker.image("${MAVEN_IMAGE}").inside("-u root -v ${WORKSPACE}/settings.xml:${SETTINGS_PATH}") {
                        // Debugging steps to check if the settings.xml file exists in the Docker container
                        sh 'ls -la /root/.m2/'
                        sh 'cat /root/.m2/settings.xml'
                        
                        // Verify Java version
                        sh 'java -version'

                        // Run Maven commands
                        withSonarQubeEnv('sonar-server') {
                            sh 'mvn clean package sonar:sonar -s /root/.m2/settings.xml'
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        failure {
            echo 'Pipeline failed.'
        }
        success {
            echo 'Pipeline succeeded.'
        }
    }
}
