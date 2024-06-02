pipeline{

    agent any

    stages{ 

        stage('sonar analasys status'){

            agent{

                docker{
                    image 'maven'
                    args '-v $HOME/.m2:/root/.m2 -v $WORKSPACE/settings.xml:/root/.m2/settings.xml'
                }
            }
            steps{
                
                script{

                    withSonarQubeEnv(credentialsId: 'sonar-tocken') {

                        sh 'mvn clean package sonar:sonar -s /root/.m2/settings.xml'
                    }
                }
            }      
        }
    }
}