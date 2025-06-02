pipeline {
  agent any

  tools {
    maven 'M298' // This must match the Maven name configured in Jenkins Global Tool Configuration
    jdk 'jdk17'

  }

  stages {
    stage('Build') {
      steps {
        echo 'Building the project...'
        sh 'mvn clean package -DskipTests=true'
        archiveArtifacts artifacts: 'target/hello-demo-*.jar', fingerprint: true
      }
    }

    stage('Test') {
      steps {
        echo 'Running unit tests...'
        sh 'mvn test'
        junit testResults: 'target/surefire-reports/TEST-*.xml', 
              allowEmptyResults: true, 
              keepProperties: true, 
              keepTestNames: true
      }
    }

    stage('Containerization') {
      steps {
        echo 'Docker Build, Tag, and Push steps go here...'
        sh '''
          echo "Building Docker image..."
          echo "Tagging Docker image..."
          echo "Pushing Docker image to registry..."
        '''
      }
    }

    stage('Kubernetes Deployment') {
      steps {
        echo 'Deploying to Kubernetes using ArgoCD...'
        sh 'echo "argocd app sync your-app-name"'
      }
    }

    stage('Integration Testing') {
      steps {
        echo 'Running integration tests...'
        sh '''
          sleep 10
          echo "Performing curl-based integration tests..."
        '''
      }
    }
  }
}
