pipeline {
    agent any

    environment {
        MAVEN_IMAGE = 'maven:3.6.3-jdk-8'
        SETTINGS_PATH = '/root/.m2/settings.xml'
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk' // Adjust this path to where Java 11 is installed on your server
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
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
                    // Adjusting the user to root to avoid permission issues
                    docker.image("${MAVEN_IMAGE}").inside("-u root -v ${WORKSPACE}/settings.xml:${SETTINGS_PATH}") {
                        // Debugging steps to check if the settings.xml file exists in the Docker container
                        sh 'ls -la /root/.m2/'
                        sh 'cat /root/.m2/settings.xml'

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