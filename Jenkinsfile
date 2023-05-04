pipeline {
  agent any

  stages {
    stage('Backup current project directory') {
      steps {
        sh 'tar -cvf todo-list_$(date +%Y-%m-%d-%H.%M.%S).tar -C /var/www todo-list'
        sh 'cd /home/ubuntu/backup && ls -t | tail -n +3 | xargs sudo rm -f'
      }
    }

    stage('Copy changes to production server') {
      steps {
        sh 'scp -pr * /var/www/todo-list'
      }
    }

    stage('Deploy with Docker Compose') {
      steps {
        sh 'cd /var/www/todo-list && docker-compose -f docker-compose.yml down'
        sh 'cd /var/www/todo-list && docker-compose -f docker-compose.yml up --build -d'
      }
    }
  }
}
