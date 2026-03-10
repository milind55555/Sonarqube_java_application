pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                    mvn sonar:sonar \
                    -Dsonar.projectKey=java-app \
                    -Dsonar.host.url=http://3.108.66.255:9000 \
                    -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
    }
}
