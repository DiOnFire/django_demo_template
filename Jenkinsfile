pipeline {
    agent {
        docker {image 'python:3.10'}
    }
    stages {
        stage('Run Test') {
            steps {
                sh '''
                    python3 -m venv .venv
                    . .venv/bin/activate
                    pip install -r requirements.txt
                    python manage.py test
                '''
            }
        }
        stage("build") {
            agent any
            steps {
                sh 'docker build . -t DiOnFire/django_demo_template:${GIT_COMMIT} -t DiOnFire/django_demo_template:latest'
            }
        }
        stage("push") {
            agent any
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'docker login -u ${USERNAME} -p ${PASSWORD}'
                    sh 'docker push DiOnFire/django_demo_template:${GIT_COMMIT}'
                    sh 'docker push DiOnFire/django_demo_template:latest'
                }
            }
        }
    }
}