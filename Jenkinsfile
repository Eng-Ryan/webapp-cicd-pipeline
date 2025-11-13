pipeline {
    agent any

    tools {
        maven 'Maven'  
    }

    environment {
        
        JAVA_OPTS = "-Dorg.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL=86400"
    }

    stages {
        stage('Initialize') {
            steps {
                sh '''
                    echo "PATH = $PATH"
                    echo "M2_HOME = $M2_HOME"
                '''
            }
        }

        stage('Pre-Fetch Dependencies') {
            steps {
                
                withEnv(["JAVA_OPTS=$JAVA_OPTS"]) {
                    sh 'mvn dependency:go-offline -B'
                }
            }
        }

        stage('Build') {
            steps {
                withEnv(["JAVA_OPTS=$JAVA_OPTS"]) {
                    sh 'mvn clean package -B -DskipTests'  
                }
            }
        }
    }

    post {
        success {
            echo "Build completed successfully!"
        }
        failure {
            echo "Build failed. Check logs for errors."
        }
    }
}
