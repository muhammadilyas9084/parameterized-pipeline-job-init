pipeline {
  agent any

  parameters {
    string(name: 'SLEEP_TIME', defaultValue: '10', description: 'Time to wait before integration test')
    string(name: 'APP_PORT', defaultValue: '8080', description: 'Port the app runs on')
    string(name: 'BRACH_NAME', defaultValue: 'main', description: 'Git branch name')
  }

  tools {
    maven 'M398'  // ✅ Make sure this matches the Maven installation in Jenkins
  }

  stages {
    stage('Maven Version') {
      steps {
        sh 'echo Print Maven Version'
        sh 'mvn -version'
        sh 'echo Sleep-Time - ${SLEEP_TIME}, Port - ${APP_PORT}, Branch - ${BRACH_NAME}'
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package -DskipTests=true'
        archiveArtifacts artifacts: 'target/hello-demo-*.jar', fingerprint: true
      }
    }

    stage('Test') {
      steps {
        sh 'mvn test'
        junit testResults: 'target/surefire-reports/TEST-*.xml', keepProperties: true, keepTestNames: true
      }
    }

    stage('Local Deployment') {
      steps {
        sh 'nohup java -jar target/hello-demo-*.jar > app.log 2>&1 &'
      }
    }

    stage('Integration Testing') {
      steps {
        sh "sleep ${SLEEP_TIME}"
        sh "curl -s http://localhost:${APP_PORT}/hello"
      }
    }
  }
}
