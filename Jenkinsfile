pipeline {
    agent any 

    environment {
        TOMCAT_USER = 'admin'
        TOMCAT_PASS = '82025840'
        TOMCAT_URL  = 'http://localhost:8080'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/Zaid8294/SEPM.git'
            }
        }

        stage('Build with Maven') {
            steps {
                bat 'mvn clean package'  // Ensure Maven is installed and in PATH
            }
        }

        stage('Run Tests') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFile = 'target/myapp.war'
                    bat """
                    curl --user %TOMCAT_USER%:%TOMCAT_PASS% --upload-file ${warFile} "%TOMCAT_URL%/manager/text/deploy?path=/myapp&update=true"
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Build or Deployment failed!'
        }
    }
}
