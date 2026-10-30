pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/AhmedRaza02912/DevOpsLab3.git'
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/laravel-k8s.yaml'
        sh 'kubectl rollout restart deployment/laravel'
        sh 'kubectl rollout status deployment/mysql --timeout=300s'
        sh 'kubectl rollout status deployment/laravel --timeout=600s'
      }
    }
    stage('Migrate Database') {
      steps {
        sh "kubectl exec deploy/laravel -c laravel -- sh -c 'cd /app && php artisan migrate --force'"
      }
    }
    stage('Verify') {
      steps {
        sh 'kubectl get pods,svc'
        sh 'curl -I http://localhost:30080 || true'
      }
    }
  }
}
