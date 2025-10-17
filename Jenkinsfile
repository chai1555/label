pipeline {
    agent any

    stages {
        // ===== FRONTEND BUILD =====
        stage('Build Frontend') {
            steps {
                dir('reactfrontend') {
                    bat 'npm install'
                    bat 'npm run build'
                }
            }
        }

        // ===== FRONTEND DEPLOY =====
        stage('Deploy Frontend to Tomcat') {
            steps {
                bat '''
                    
                    if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\reactfrontend" (
                        rmdir /S /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\reactfrontend"
                    )

                    REM --- Create new frontend folder in Tomcat ---
                    mkdir "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\reactfrontend"

                    REM --- Copy build output ---
                    xcopy /E /I /Y reactfrontend\\dist\\* "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\reactfrontend"
                '''
            }
        }

        // ===== BACKEND BUILD =====
        stage('Build Backend') {
            steps {
                dir('springbootbackendend') {
                    bat 'mvn clean package -DskipTests'
                }
            }
        }

        // ===== BACKEND DEPLOY =====
        stage('Deploy Backend to Tomcat') {
            steps {
                bat '''
                    if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\springapp.war" (
                        del "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\springapp.war"
                    )
                    if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\springapp.war" (
                        rmdir /S /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\springapp.war"
                    )
                    copy springbootbackend\\target\\movieapp.war "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\"
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully."
        }
        failure {
            echo "Pipeline Failed. Check folder names and logs."
        }
    }
}
