pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME'     // Use Maven installed in Jenkins (define in Global Tools)
    }

    environment {
        WAR_NAME = "html-webapp.war"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/ishwarbarhate/html.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Archive WAR') {
            steps {
                archiveArtifacts artifacts: "target/*.war", fingerprint: true
            }
        }

        // OPTIONAL: Deploy to Tomcat
        stage('Deploy to Tomcat') {
            when {
                expression { fileExists("target/${env.WAR_NAME}") }
            }
            steps {
                script {
                    def tomcatUrl = "http://localhost:8080/manager/text/deploy?path=/html-webapp&update=true"
                    def warFile = "target/${env.WAR_NAME}"
                    withCredentials([usernamePassword(credentialsId: 'tomcat-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        sh """
                            curl -u $USER:$PASS -T ${warFile} "${tomcatUrl}"
                        """
                    }
                }
            }
        }
    }
}
