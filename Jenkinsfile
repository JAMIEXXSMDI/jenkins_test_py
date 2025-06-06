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
                    // Копирование файлов на удаленный сервер
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'my-ssh-server',  // Имя SSH-сервера (настроено в Jenkins)
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: '**/*',  // Копируем все файлы
                                        removePrefix: '',     // Не обрезаем пути
                                        remoteDirectory: "app-${params.ENV}",  // Папка на сервере (app-dev или app-prod)
                                        execCommand: """
                                            echo "Файлы успешно скопированы в /var/www/app-${params.ENV}"
                                            ls -la /var/www/app-${params.ENV}  # Проверка содержимого
                                        """
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}
