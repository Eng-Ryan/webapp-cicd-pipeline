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
                sh 'mvn clean package'
            }
        }

        stage ('Deploy-To-Tomcat') {
            steps {
                sshagent(['tomcat']) {
                    sh '''
                       echo "[*] Uploading WAR file..."
                       scp -o StrictHostKeyChecking=no target/WebApp.war ubuntu@54.82.35.226:/home/ubuntu/prod/apache-tomcat-9.0.112/webapps/webapp.war
                       echo "[*] Deployment complete!"
                    '''
        }      
    }       
}


        
    }
}
