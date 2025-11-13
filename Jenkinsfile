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
        stage ('Check-Git-Secrets') {
            steps {
                sh 'rm trufflehog || true'
                sh 'docker run gesellix/trufflehog --json https://github.com/Eng-Ryan/webapp-cicd-pipeline.git > trufflehog'
                sh 'cat trufflehog'
            }
        }

            stage ('Source Composition Analysis') {
                steps {
                    sh 'rm owasp* || true'
                    sh 'wget "https://raw.githubusercontent.com/Eng-Ryan/webapp-cicd-pipeline/refs/heads/main/owasp-dependency-check.sh" '
                    sh 'chmod +x owasp-dependency-check.sh'
                    sh 'bash owasp-dependency-check.sh'
                    sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
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
