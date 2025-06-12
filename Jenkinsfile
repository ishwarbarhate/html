pipeline {
    agent any

    tools {
        maven 'Maven 3'  // Or the name you configured in Jenkins > Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/ishwarbarhate/html.git'
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
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
