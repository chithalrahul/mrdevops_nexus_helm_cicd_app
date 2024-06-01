pipeline{

    agent any

    stages{ 

        stage{'sonar analasys status'}{

            agent{

                docker{
                    image 'maven'
                }
            }
            steps{
                
                script{

                    withSonarQubeEnv(credentialsId: 'sonar-tocken') {

                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }      
        }
    }
}