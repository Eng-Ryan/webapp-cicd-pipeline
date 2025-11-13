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

         stage ('Deploy-To-Tomcat') {
            steps {
                sshagent(['tomcat']) {
                    sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.82.35.226:/prod/apache-tomcat-8.5.39/webapps/webapp.war'
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
