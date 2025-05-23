pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'MAVEN_HOME'
        DEPLOY_DIR = '/home/ubuntu/apache-tomcat-10.1.41/webapps'
        TOMCAT_HOME = '/home/ubuntu/apache-tomcat-10.1.41'
    }

    stages {
        stage('Clone') {
            steps {
                echo "🔄 Cloning repository..."
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

                    if [ ! -d "$DEPLOY_DIR" ]; then
                        echo "📁 Creating deployment directory $DEPLOY_DIR"
                        sudo mkdir -p "$DEPLOY_DIR"
                    fi

                    if [ -f "$WAR_FILE" ]; then
                        echo "📦 Copying WAR file to $DEPLOY_DIR"
                        sudo cp "$WAR_FILE" "$DEPLOY_DIR/"
                        echo "✅ WAR file copied successfully."

                        echo "🔄 Restarting Tomcat server..."
                        sudo $TOMCAT_HOME/bin/shutdown.sh || true
                        sleep 5
                        sudo $TOMCAT_HOME/bin/startup.sh
                        echo "🚀 Tomcat restarted."
                    else
                        echo "❌ WAR file not found!"
                        exit 1
                    fi
                '''
            }
        }
    }

    post {
        failure {
            echo "❌ Deployment failed!"
        }
        success {
            echo "✅ Pipeline completed successfully!"
        }
    }
}

