pipeline {
    agent any

    tools {
        maven "M2_HOME"
    }

        environment {
        DOCKER_CREDENTIALS= credentials('dockerhub')
    }

    stages {
        stage("Code Checkout") {
            steps {
                git branch: 'master',
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
         stage('Docker Build') {
            steps {
                script {
                    sh "docker build -t yasmine2004/student-management:1.0 ."
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    sh 'echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin'
                    sh "docker push yasmine2004/student-management:1.0"
                }
            }
        }

        stage('Deploy to Kubernetes') {
    steps {
        script {
            // Applique le fichier multi-doc YAML
            sh "kubectl apply -f k8s-deployment.yaml -n devopss"

            sh "kubectl rollout status deployment/spring-app -n devopss"
        }
    }
}

post {
    always {
    
        sh "kubectl get pods -n devopss"
    }
}


    }
}
