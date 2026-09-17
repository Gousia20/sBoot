

pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
              checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.war',
                                 fingerprint: true
            }
        }

        // CD - Deployment

        stage('Tomcat Deploy') {
            steps {
                sh '''
                    echo "Stopping Tomcat"

                    sudo /home/gousia/Desktop/tomcat/tomcat/bin/shutdown.sh || true

                    sleep 5

                    echo "Removing old WAR"

                    sudo rm -rf /home/gousia/Desktop/tomcat/tomcat/webapps/spring-boot-rest-example-0.5.0.war
                    sudo rm -rf /home/gousia/Desktop/tomcat/tomcat/webapps/spring-boot-rest-example-0.5.0

                    echo "Deploying new WAR"

                    sudo cp target/spring-boot-rest-example-0.5.0.war \
                        /home/gousia/Desktop/tomcat/tomcat/webapps/

                    echo "Starting Tomcat"

                    sudo /home/gousia/Desktop/tomcat/tomcat/bin/startup.sh

                    sleep 10
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    curl -f http://localhost:8081/spring-boot-rest-example-0.5.0/example/v1/hotels
                '''
            }
        }
    }

    post {
        success {
            echo 'sBoot deployment successful!'
        }

        failure {
            echo 'sBoot deployment failed!'
        }
    }
}





    
