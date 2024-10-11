pipeline {
    agent any
    environment {
        TELEPUSH_TOKEN = '2f10a2'
        ALERTMANAGER_YML_FILE = 'alertmanager.yml'
        ALERTMANAGER_YML_PATH = '/home/ubuntu/grafan/alertmanager.yml'
        ALERT_RULES_YML_FILE = 'alert.rules.yml'
        ALERT_RULES_YML_PATH = '/home/ubuntu/grafan/alert.rules.yml'
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Якщо використовуєте Git SCM, клонування відбувається автоматично, тут можна додати додаткові дії
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
                cd /home/ubuntu/grafan
                sudo docker swarm init
                sudo docker compose up -d
                '''
            }
        }
    }
}
