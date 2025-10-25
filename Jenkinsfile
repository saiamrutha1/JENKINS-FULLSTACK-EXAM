pipeline {
    agent any

    stages {

        // ===== FRONTEND BUILD =====
        stage('Build Frontend') {
            steps {
                dir('hospital-frontend') {
                    bat 'npm install'
                    bat 'npm run build'
                }
            }
        }

        // ===== FRONTEND DEPLOY =====
        stage('Deploy Frontend to Tomcat') {
            steps {
                bat '''
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps\\hospital-exam" (
                    rmdir /S /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps\\hospital-exam"
                )
                mkdir "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps\\hospital-exam"
                xcopy /E /I /Y hospital-frontend\\dist\\* "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps\\hospital-exam"
                '''
            }
        }

        // ===== BACKEND BUILD =====
        stage('Build Backend') {
            steps {
                dir('hospital-backend-jenkins') {
                    bat 'mvn clean package'
                }
            }
        }

        // ===== BACKEND DEPLOY =====
        stage('Deploy Backend to Tomcat') {
            steps {
                bat '''
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps\\hospital-backend-exam.war" (
                    del /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps\\hospital-backend-exam.war"
                )
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps\\hospital-backend-exam" (
                    rmdir /S /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps\\hospital-backend-exam"
                )
                copy "hospital-backend-jenkins\\target\\*.war" "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\webapps\\"
                '''
            }
        }

    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Pipeline Failed.'
        }
    }
}