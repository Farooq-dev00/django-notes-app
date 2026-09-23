pipeline {
    agent any

    environment {
        APP_PORT = "8000"
    }

    stages {

        stage('Clone Repo') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Umair1012/django-notes-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv venv
                    ./venv/bin/pip install --upgrade pip
                    ./venv/bin/pip install -r requirements.txt
                    ./venv/bin/pip install --upgrade gunicorn
                '''
            }
        }

        stage('Run Django with Gunicorn') {
            steps {
                sh '''
                    pkill -f '[g]unicorn.*8000' || true

                    JENKINS_NODE_COOKIE=dontKillMe \
                    nohup ./venv/bin/gunicorn \
                    --bind 0.0.0.0:$APP_PORT \
                    notesapp.wsgi:application \
                    > gunicorn.log 2>&1 < /dev/null &

                    sleep 5

                    curl http://127.0.0.1:$APP_PORT
                '''
            }
        }
    }
}

