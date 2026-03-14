pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/YOUR_GITHUB_USERNAME/YOUR_REPO_NAME.git'
            }
        }

        stage('Deploy Portfolio Website') {
            steps {
                sh '''
                    sudo mkdir -p /var/www/techprerana
                    sudo rsync -av --delete \
                      --exclude='.git' \
                      --exclude='Jenkinsfile' \
                      --exclude='README.MD' \
                      --exclude='screenshots' \
                      ./ /var/www/techprerana/
                '''
            }
        }

        stage('Set Permissions') {
            steps {
                sh '''
                    sudo chown -R www-data:www-data /var/www/techprerana
                    sudo chmod -R 755 /var/www/techprerana
                '''
            }
        }

        stage('Reload Nginx') {
            steps {
                sh 'sudo systemctl reload nginx'
            }
        }
    }
}