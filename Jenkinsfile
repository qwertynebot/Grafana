pipeline {
    agent any
    environment {
        TELEPUSH_TOKEN = '2f10a2'
        ALERTMANAGER_YML_FILE = 'alertmanager.yml'
        ALERTMANAGER_YML_PATH = '/var/lib/jenkins/workspace/Grafana/alertmanager.yml'  // Оновлений шлях
        ALERT_RULES_YML_FILE = 'alert.rules.yml'
        ALERT_RULES_YML_PATH = '/var/lib/jenkins/workspace/Grafana/alert.rules.yml'   // Оновлений шлях
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Використання Git SCM для автоматичного клонування
                checkout scm
            }
        }
        stage('Update Configuration Files') {
            steps {
                script {
                    // Оновлюємо alertmanager.yml з токеном
                    def alertmanagerFilePath = "${ALERTMANAGER_YML_FILE}"
                    def content = readFile(file: alertmanagerFilePath)
                    content = content.replace('your-token', TELEPUSH_TOKEN)
                    writeFile(file: alertmanagerFilePath, text: content)

                    // Оновлюємо docker-compose.yml з правильними шляхами
                    def dockerComposeFilePath = 'docker-compose.yml'
                    def dockerComposeContent = readFile(file: dockerComposeFilePath)
                    dockerComposeContent = dockerComposeContent.replace('/your/path/alertmanager.yml', ALERTMANAGER_YML_PATH)
                    dockerComposeContent = dockerComposeContent.replace('/your/path/alert.rules.yml', ALERT_RULES_YML_PATH)
                    writeFile(file: dockerComposeFilePath, text: dockerComposeContent)
                }
            }
        }
        stage('Build and Run') {
            steps {
                sh '''
                cd /var/lib/jenkins/workspace/Grafana
                sudo docker swarm init
                sudo docker compose up -d
                '''
            }
        }
    }
}
