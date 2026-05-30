pipeline {

    agent any

    environment {
        SERVER = "ubuntu@13.126.216.0"
        WEB_DIR = "/var/www/html"
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/CoderWithSharingan/Automated-React-Application-Deployment-Using-Jenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                scp -r dist/* $SERVER:/tmp/

                ssh $SERVER "
                sudo rm -rf $WEB_DIR/*

                sudo cp -r /tmp/* $WEB_DIR/

                sudo systemctl restart nginx
                "
                '''
            }
        }
    }

    post {

        success {
            echo "React Deployment Successful"
        }

        failure {
            echo "Deployment Failed"
        }
    }
}
