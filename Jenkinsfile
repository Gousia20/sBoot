pipeline {
    agent any
    stages {


      stage('checkout') {
        steps {
          git branch: 'freature/gousia' , url: 'git@github.com:Gousia20/sBoot.git'
} 
 }
      stage('build') {
        steps {
          sh 'mvn clean package -DskipTests'
}
 }

      stage('Test') {
        steps {
          sh 'mvn test'
}
 }

      stage('archive') {
        steps {
          achiveArtifacts artifacts: 'target/*.war'
            fingureprint= true
}
  }

 # CD (deployment)

      stage('tomcat deploy') {
        steps {
          sh ' ' '
            echo "stopping tomcat"
            sudo /home/gousia/Desktop/tomcat/tomcat/bin/shutdown.sh || true
            
            sleep 5
            
            echo "removing old war"
            sudo rm -rf  /home/gousia/Desktop/tomcat/tomcat/webapps/spring-boot-rest-example-0.5.0.war

            echo "deploying the new war"
            sudo cp /target/spring-boot-rest-example-0.5.0.war \ /home/gousia/Desktop/tomcat/tomcat/webapps


            echo "starting tomcat"
            sudo /home/gousia/Desktop/tomcat/tomcat/bin/startup.sh

            sleep 10

            ' ' ' 
}           
 }
           stage('verify') {
             steps {
               sh 'curl -f \ http://localhost:8081/spring-boot-rest-example-0.5.0/example/v1/hotels'
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








    
