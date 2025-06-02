pipeline {
  agent any

  // 1. Add your parameters here
  parameters {
    string(name: 'SLEEP_TIME', defaultValue: '10', description: 'Time to wait before integration test')
    string(name: 'APP_PORT', defaultValue: '8080', description: 'Application port number')
    string(name: 'BRANCH_NAME', defaultValue: 'test', description: 'Git branch name')
  }

  // 2. Add Java (JDK) and Maven tools here
  tools {
    jdk 'jdk17'             // <-- This must match the name in Jenkins Global Tool Configuration
    maven 'M398'            // <-- This must match your configured Maven tool name
  }

  stages {
    stage('Maven Version') {
      steps {
        sh 'echo Print Maven Version'
        sh 'mvn -version'
        // 3. Correct the variable spelling: BRANCH_NAME (not BRACH_NAME)
        sh "echo Sleep-Time - ${params.SLEEP_TIME}, Port - ${params.APP_PORT}, Branch - ${params.BRANCH_NAME}"
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package -DskipTests=true'
        archiveArtifacts 'target/hello-demo-*.jar'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn test'
        junit(testResults: 'target/surefire-reports/TEST-*.xml', keepProperties: true, keepTestNames: true)
      }
    }

    stage('Local Deployment') {
      steps {
        // Use nohup to keep the process running in background
        sh 'nohup java -jar target/hello-demo-*.jar > /dev/null 2>&1 &'
      }
    }

    stage('Integration Testing') {
      steps {
        sh "sleep ${params.SLEEP_TIME}"
        sh "curl -s http://localhost:${params.APP_PORT}/hello"
      }
    }
  }
}
