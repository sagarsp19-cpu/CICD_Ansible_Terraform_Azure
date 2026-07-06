pipeline {
    agent any

    tools {
        maven 'Maven-3.8.7'
        jdk 'JDK-17'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
                junit '**/target/surefire-reports/TEST-*.xml'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Deploy') {
         steps {
        sh '''
        pkill -f your-app-0.0.1-SNAPSHOT.jar || true

        nohup java -jar target/your-app-0.0.1-SNAPSHOT.jar \
        --server.port=8081 \
        --server.address=0.0.0.0 \
        > app.log 2>&1 &
        '''
         }
      }
        stage('Deploy using Ansible') {
    steps {
        sh '''
        ansible-playbook \
        -i ansible/environments/test/hosts \
        ansible/playbooks/deploy.yml
        '''
    }
}
    }
}
