pipeline {
    agent any

    tools {
        maven 'Maven 3'  // Make sure this matches Jenkins Maven name
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ishwarbarhate/html.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Deploy') {
            steps {
                sh 'docker rm -f html-webapp || true'
                sh """
                    docker run -d \
                    --name html-webapp \
                    -p 8080:8080 \
                    -v ${WORKSPACE}/target/html-webapp.war:/usr/local/tomcat/webapps/html-webapp.war \
                    tomcat:9.0
                """
            }
        }
    }

    post {
        success {
            echo '✅ Build and deployment succeeded!'
        }
        failure {
            echo '❌ Build or deployment failed.'
        }
    }
}
