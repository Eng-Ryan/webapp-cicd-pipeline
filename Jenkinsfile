pipeline {
    agent any

    tools {
        maven 'Maven'
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
        
        stage('Build') {
            steps {
                sh '''
                    echo "[*] Starting Maven Build..."
                    mvn clean --batch-mode -Dorg.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL=86400
                    mvn package --batch-mode -Dorg.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL=86400
                    echo "[*] Build Finished!"
                '''
            }
        }


    stage ('Deploy-To-Tomcat') {
            steps {
                sshagent(['tomcat']) {
                    sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.82.35.226:/home/ubuntu/prod/apache-tomcat-9.0.112/webapps/webapp.war'
                }
            }
    }
 }
}
