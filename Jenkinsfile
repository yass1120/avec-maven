pipeline {
    agent any

    tools {
        maven "M2_HOME"
    }

    stages {
        stage("Code Checkout") {
            steps {
                git branch: 'main',
                    url: 'https://github.com/yass1120/avec-maven.git'
            }
        }

        stage('Code Test') {
            steps {
                sh "mvn test"
            }
        }

        stage('Code Build') {
            steps {
                sh "mvn package"
            }
        }

        stage('Sonar Test'){
            steps {
                withSonarQubeEnv('SQ1') {
                    sh "mvn sonar:sonar"
                }
            }
        }
    }
}
