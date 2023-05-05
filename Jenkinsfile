pipeline {
  agent any

  stages {
    stage('Backup current project directory') {
      steps {
        sh 'tar -cvf todo-list_$(date +%Y-%m-%d-%H.%M.%S).tar -C /var/www todo-list'
        sh 'sudo mv todo-list*.tar /var/lib/jenkins/backup'
        sh 'cd /var/lib/jenkins/backup && ls -t | tail -n +3 | xargs sudo rm -f'
      }
    }

    stage('Copy changes to production server') {
      steps {
        sh 'cp -pr * /var/www/todo-list'
      }
    }

    stage('Deploy with Docker Compose') {
      steps {
        sh 'cd /var/www/todo-list && docker compose -f docker-compose.yaml down'
        sh 'cd /var/www/todo-list && docker compose -f docker-compose.yaml up --build -d'
      }
    }
  }
}
