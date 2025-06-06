pipeline {
    agent any

    // Параметр для выбора окружения
    parameters {
        choice(
            name: 'ENV',
            choices: ['dev', 'prod'],
            description: 'Выберите окружение для деплоя'
        )
    }

    stages {
        // Этап 1: Подготовка
        stage('Prepare') {
            steps {
                echo "Deploying to ${params.ENV}"
                cleanWs()  // Очистка рабочей директории
            }
        }

        // Этап 2: Деплой через SSH
        stage('Deploy') {
    steps {
        script {
            try {
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'my-ssh-server',
                            transfers: [
                                sshTransfer(
                                    sourceFiles: '**/*',
                                    remoteDirectory: "app-${params.ENV}",
                                    execCommand: """
                                        echo 'Деплой в ${params.ENV} успешен!'
                                        ls -la /var/www/app-${params.ENV}
                                    """
                                )
                            ]
                        )
                    ]
                )
            } catch (Exception e) {
                echo "Ошибка деплоя: ${e.message}"
                currentBuild.result = 'UNSTABLE'
            }
        }
    }
}
    }
}
