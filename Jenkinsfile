pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Build Success'
                }
                failure {
                    echo 'Build Failed'
                }
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                success {
                    echo 'Test Success'
                }
                failure {
                    echo 'Test Failed'
                }
            }
        }

        stage('Deploy Spring Boot') {
            steps {
                sh '''
                echo "Stopping old application..."
                sudo pkill -f spring_app_sak-0.0.1-SNAPSHOT.jar || true

                sleep 5

                echo "Starting application..."
                nohup sudo java -jar target/spring_app_sak-0.0.1-SNAPSHOT.jar > spring.log 2>&1 &

                sleep 15

                echo "Checking application..."

                if pgrep -f spring_app_sak-0.0.1-SNAPSHOT.jar > /dev/null
                then
                    echo "Spring Boot started successfully."
                else
                    echo "Spring Boot failed to start."
                    cat spring.log
                    exit 1
                fi
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}