pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'MAVEN_HOME'
        DEPLOY_DIR = '/opt/tomcat/webapps'
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/your-username/java-webapp.git'
            }
        }

        stage('Build') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean package"
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying WAR file to Tomcat..."
                sh '''
                    WAR_FILE=$(ls target/*.war | head -n 1)
                    sudo cp $WAR_FILE $DEPLOY_DIR
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment complete!'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}

