pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME'    // must match your Maven install name in Jenkins → Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ishwarbarhate/html.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Archive WAR') {
            steps {
                // This will keep your WAR as a build artifact in Jenkins
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "✅ .war built and archived successfully!"
        }
        failure {
            echo "❌ Build failed."
        }

        stage('Docker Deploy') {
  steps {
    // Remove any old container
    sh 'docker rm -f html-webapp || true'
    // Launch Tomcat with your WAR mounted
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
}

