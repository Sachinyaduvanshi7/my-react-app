pipeline {
    agent any

    environment {
        REACT_APP_DIR = 'your-react-app-folder'  // Update if not root
        EC2_USER = 'ubuntu'  // or ubuntu
        EC2_HOST = '13.201.227.220'
        PEM_FILE = '/path/to/your/key.pem'
        REMOTE_DIR = '/var/www/html' // or your desired path
    }

    stages {
        stage('Clone') {
            steps {
                git credentialsId: 'your-github-token-id', url: 'https://github.com/your/repo.git', branch: 'main'
            }
        }

        stage('Install & Build') {
            steps {
                dir("${env.REACT_APP_DIR}") {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh """
                ssh -o StrictHostKeyChecking=no -i ${PEM_FILE} ${EC2_USER}@${EC2_HOST} 'rm -rf ${REMOTE_DIR} && mkdir -p ${REMOTE_DIR}'
                scp -i ${PEM_FILE} -r ${REACT_APP_DIR}/build/* ${EC2_USER}@${EC2_HOST}:${REMOTE_DIR}/
                """
            }
        }
    }
}
