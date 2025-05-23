pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'MAVEN_HOME'
        DEPLOY_DIR = '/home/ubuntu/apache-tomcat-10.1.41/webapps'
    }

    stages {
        stage('Clone') {
            steps {
                git credentialsId: 'github-token',
                    url: 'https://github.com/Soni-ak/java-webapp.git',
                    branch: 'main'
            }
        }

        stage('Build') {
            steps {
                echo "🔨 Building project using Maven..."
                sh "${MAVEN_HOME}/bin/mvn clean package"
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Deploying WAR file to Tomcat..."
                sh '''
                    WAR_FILE=$(ls target/*.war | head -n 1)
                    if [ -f "$WAR_FILE" ]; then
                        if [ ! -d "$DEPLOY_DIR" ]; then
                            echo "Creating deployment directory $DEPLOY_DIR"
                            mkdir -p $DEPLOY_DIR
                        fi
                        sudo cp "$WAR_FILE" $DEPLOY_DIR/
                        echo "WAR file deployed successfully."
                    else
                        echo "WAR file not found!"
                        exit 1
                    fi
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

