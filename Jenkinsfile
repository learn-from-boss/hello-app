pipeline {
    agent any

    environment {
        SONARQUBE_SCANNER_HOME = tool 'MySonarQube'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/learn-from-boss/hello-app.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('MySonarQube') {
                    sh 'mvn clean verify sonar:sonar'
                }
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Publish to Nexus') {
            steps {

                configFileProvider([configFile(fileId: '475043e5-9b06-4ab5-82a0-213e1167069a', variable: 'nexus')]) {
                     sh 'mvn -s /home/boss/.m2/settings.xml deploy -DskipTests=true'
                }
               
            }
        }
    }
}
